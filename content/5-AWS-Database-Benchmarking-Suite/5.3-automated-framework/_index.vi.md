---
title : "Xây dựng Automated Testing Framework"
date : "2025-06-08"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

#### Xây dựng Automated Testing Framework

#### Tổng quan
Trong lab này, bạn sẽ xây dựng một automated testing framework hoàn chỉnh để thực hiện các benchmark tests một cách tự động.

#### Mục tiêu
- Xây dựng core benchmark engine
- Tạo database connectors cho các AWS services
- Implement metrics collection system
- Tạo test orchestration và scheduling

#### Thời gian: 60 phút

#### Bước 1: Tạo Core Benchmark Engine

```python
# benchmark_engine.py
import time
import threading
import logging
from datetime import datetime
from typing import Dict, List, Any
import boto3
import json

class BenchmarkEngine:
    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.logger = self._setup_logging()
        self.results = []
        self.test_runners = {}
        
    def _setup_logging(self):
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
            handlers=[
                logging.FileHandler('benchmark.log'),
                logging.StreamHandler()
            ]
        )
        return logging.getLogger(__name__)
    
    def register_test_runner(self, database_type: str, runner):
        self.test_runners[database_type] = runner
        self.logger.info(f"Registered test runner for {database_type}")
    
    def run_benchmark(self, test_config: Dict[str, Any]) -> Dict[str, Any]:
        test_id = f"{test_config['database_type']}_{test_config['workload']}_{datetime.now().strftime('%Y%m%d_%H%M%S')}"
        
        self.logger.info(f"Starting benchmark test: {test_id}")
        
        # Initialize test environment
        self._prepare_test_environment(test_config)
        
        # Run warmup phase
        self._run_warmup(test_config)
        
        # Execute main test
        start_time = time.time()
        test_results = self._execute_test(test_config)
        end_time = time.time()
        
        # Compile results
        benchmark_result = {
            "test_id": test_id,
            "database_type": test_config["database_type"],
            "workload": test_config["workload"],
            "start_time": start_time,
            "end_time": end_time,
            "duration": end_time - start_time,
            "test_results": test_results,
            "configuration": test_config
        }
        
        self.results.append(benchmark_result)
        self.logger.info(f"Completed benchmark test: {test_id}")
        
        return benchmark_result
    
    def _prepare_test_environment(self, test_config: Dict[str, Any]):
        database_type = test_config["database_type"]
        if database_type in self.test_runners:
            self.test_runners[database_type].prepare_environment(test_config)
    
    def _run_warmup(self, test_config: Dict[str, Any]):
        warmup_duration = test_config.get("warmup_time", 60)
        self.logger.info(f"Running warmup for {warmup_duration} seconds...")
        time.sleep(warmup_duration)
    
    def _execute_test(self, test_config: Dict[str, Any]) -> Dict[str, Any]:
        database_type = test_config["database_type"]
        if database_type not in self.test_runners:
            raise ValueError(f"No test runner registered for {database_type}")
        
        return self.test_runners[database_type].run_test(test_config)
    
    def save_results(self, filename: str = None):
        if filename is None:
            filename = f"benchmark_results_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json"
        
        with open(filename, 'w') as f:
            json.dump(self.results, f, indent=2, default=str)
        
        self.logger.info(f"Results saved to {filename}")
```

#### Bước 2: Tạo MySQL Test Runner

```python
# mysql_test_runner.py
import mysql.connector
from mysql.connector import pooling
from typing import Dict, Any, List
import random
import time
import threading
import queue

class MySQLTestRunner:
    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.connection_pool = None
    
    def prepare_environment(self, test_config: Dict[str, Any]):
        pool_config = {
            "host": test_config["mysql"]["endpoint"],
            "port": test_config["mysql"]["port"],
            "user": test_config["mysql"]["username"],
            "password": test_config["mysql"]["password"],
            "database": test_config["mysql"]["database"],
            "pool_name": "benchmark_pool",
            "pool_size": test_config.get("max_connections", 50),
            "autocommit": True
        }
        
        self.connection_pool = mysql.connector.pooling.MySQLConnectionPool(**pool_config)
        self._create_test_tables()
        self._load_test_data(test_config)
    
    def _create_test_tables(self):
        connection = self.connection_pool.get_connection()
        cursor = connection.cursor()
        
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS users (
                id INT AUTO_INCREMENT PRIMARY KEY,
                name VARCHAR(255) NOT NULL,
                email VARCHAR(255) UNIQUE NOT NULL,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                status ENUM('active', 'inactive', 'pending') DEFAULT 'active',
                INDEX idx_email (email),
                INDEX idx_status (status)
            )
        """)
        
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id INT AUTO_INCREMENT PRIMARY KEY,
                name VARCHAR(255) NOT NULL,
                category VARCHAR(100) NOT NULL,
                price DECIMAL(10,2) NOT NULL,
                stock INT DEFAULT 0,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                INDEX idx_category (category),
                INDEX idx_price (price)
            )
        """)
        
        connection.commit()
        cursor.close()
        connection.close()
    
    def _load_test_data(self, test_config: Dict[str, Any]):
        data_size = test_config.get("data_size", "medium")
        sizes = {"small": 1000, "medium": 10000, "large": 100000}
        num_users = sizes.get(data_size, 10000)
        
        connection = self.connection_pool.get_connection()
        cursor = connection.cursor()
        
        for i in range(num_users):
            cursor.execute("""
                INSERT INTO users (name, email, status) 
                VALUES (%s, %s, %s)
            """, (f"User_{i}", f"user{i}@example.com", "active"))
        
        connection.commit()
        cursor.close()
        connection.close()
    
    def run_test(self, test_config: Dict[str, Any]) -> Dict[str, Any]:
        workload = test_config["workload"]
        concurrent_users = test_config["concurrent_users"]
        duration = test_config["duration"]
        
        operations = self._generate_operations(workload, concurrent_users)
        results = self._execute_concurrent_operations(operations, duration)
        
        return self._compile_results(results)
    
    def _generate_operations(self, workload: str, concurrent_users: int) -> List[Dict[str, Any]]:
        operations = []
        
        if workload == "OLTP_Standard":
            for i in range(concurrent_users):
                if random.random() < 0.7:  # 70% reads
                    operations.append({
                        "type": "SELECT",
                        "query": "SELECT * FROM users WHERE id = %s",
                        "params": [random.randint(1, 10000)]
                    })
                else:  # 30% writes
                    operations.append({
                        "type": "INSERT",
                        "query": "INSERT INTO users (name, email) VALUES (%s, %s)",
                        "params": [f"NewUser_{i}", f"newuser{i}@example.com"]
                    })
        
        return operations
    
    def _execute_concurrent_operations(self, operations: List[Dict[str, Any]], duration: int) -> List[Dict[str, Any]]:
        results = []
        start_time = time.time()
        
        def worker(operation_queue, results_queue):
            while time.time() - start_time < duration:
                try:
                    operation = operation_queue.get_nowait()
                    result = self._execute_operation(operation)
                    results_queue.put(result)
                except queue.Empty:
                    break
        
        operation_queue = queue.Queue()
        for operation in operations:
            operation_queue.put(operation)
        
        results_queue = queue.Queue()
        
        threads = []
        for i in range(len(operations)):
            thread = threading.Thread(target=worker, args=(operation_queue, results_queue))
            thread.start()
            threads.append(thread)
        
        for thread in threads:
            thread.join()
        
        while not results_queue.empty():
            results.append(results_queue.get())
        
        return results
    
    def _execute_operation(self, operation: Dict[str, Any]) -> Dict[str, Any]:
        start_time = time.time()
        
        try:
            connection = self.connection_pool.get_connection()
            cursor = connection.cursor()
            
            cursor.execute(operation["query"], operation["params"])
            
            if operation["type"] == "SELECT":
                result = cursor.fetchall()
            else:
                connection.commit()
                result = cursor.rowcount
            
            cursor.close()
            connection.close()
            
            end_time = time.time()
            
            return {
                "success": True,
                "operation": operation["type"],
                "start_time": start_time,
                "end_time": end_time,
                "duration": end_time - start_time,
                "result": result
            }
        except Exception as e:
            end_time = time.time()
            return {
                "success": False,
                "operation": operation["type"],
                "start_time": start_time,
                "end_time": end_time,
                "duration": end_time - start_time,
                "error": str(e)
            }
    
    def _compile_results(self, results: List[Dict[str, Any]]) -> Dict[str, Any]:
        successful_ops = [r for r in results if r["success"]]
        failed_ops = [r for r in results if not r["success"]]
        
        if not successful_ops:
            return {"error": "No successful operations"}
        
        durations = [r["duration"] for r in successful_ops]
        
        return {
            "total_operations": len(results),
            "successful_operations": len(successful_ops),
            "failed_operations": len(failed_ops),
            "success_rate": len(successful_ops) / len(results),
            "average_duration": sum(durations) / len(durations),
            "min_duration": min(durations),
            "max_duration": max(durations),
            "p50_duration": sorted(durations)[len(durations) // 2],
            "p95_duration": sorted(durations)[int(len(durations) * 0.95)],
            "p99_duration": sorted(durations)[int(len(durations) * 0.99)],
            "operations_per_second": len(successful_ops) / sum(durations)
        }
```

#### Bước 3: Tạo DynamoDB Test Runner

```python
# dynamodb_test_runner.py
import boto3
from botocore.exceptions import ClientError
from typing import Dict, Any, List
import random
import time
import threading
import queue

class DynamoDBTestRunner:
    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.dynamodb = boto3.resource('dynamodb', region_name=config['dynamodb']['region'])
        self.table = None
    
    def prepare_environment(self, test_config: Dict[str, Any]):
        table_name = test_config["dynamodb"]["table_name"]
        self.table = self.dynamodb.Table(table_name)
        self._load_test_data(test_config)
    
    def _load_test_data(self, test_config: Dict[str, Any]):
        data_size = test_config.get("data_size", "medium")
        sizes = {"small": 1000, "medium": 10000, "large": 100000}
        num_items = sizes.get(data_size, 10000)
        
        with self.table.batch_writer() as batch:
            for i in range(num_items):
                item = {
                    'id': f"item_{i}",
                    'name': f"Item {i}",
                    'category': random.choice(['electronics', 'clothing', 'books']),
                    'price': random.randint(10, 1000),
                    'stock': random.randint(0, 100),
                    'created_at': int(time.time())
                }
                batch.put_item(Item=item)
    
    def run_test(self, test_config: Dict[str, Any]) -> Dict[str, Any]:
        workload = test_config["workload"]
        concurrent_users = test_config["concurrent_users"]
        duration = test_config["duration"]
        
        operations = self._generate_operations(workload, concurrent_users)
        results = self._execute_concurrent_operations(operations, duration)
        
        return self._compile_results(results)
    
    def _generate_operations(self, workload: str, concurrent_users: int) -> List[Dict[str, Any]]:
        operations = []
        
        if workload == "NoSQL_High_Throughput":
            for i in range(concurrent_users):
                if random.random() < 0.8:  # 80% reads
                    operations.append({
                        "type": "GetItem",
                        "key": {"id": f"item_{random.randint(1, 10000)}"}
                    })
                else:  # 20% writes
                    operations.append({
                        "type": "PutItem",
                        "item": {
                            "id": f"new_item_{i}_{int(time.time())}",
                            "name": f"New Item {i}",
                            "category": random.choice(['electronics', 'clothing', 'books']),
                            "price": random.randint(10, 1000),
                            "stock": random.randint(0, 100),
                            "created_at": int(time.time())
                        }
                    })
        
        return operations
    
    def _execute_concurrent_operations(self, operations: List[Dict[str, Any]], duration: int) -> List[Dict[str, Any]]:
        results = []
        start_time = time.time()
        
        def worker(operation_queue, results_queue):
            while time.time() - start_time < duration:
                try:
                    operation = operation_queue.get_nowait()
                    result = self._execute_operation(operation)
                    results_queue.put(result)
                except queue.Empty:
                    break
        
        operation_queue = queue.Queue()
        for operation in operations:
            operation_queue.put(operation)
        
        results_queue = queue.Queue()
        
        threads = []
        for i in range(len(operations)):
            thread = threading.Thread(target=worker, args=(operation_queue, results_queue))
            thread.start()
            threads.append(thread)
        
        for thread in threads:
            thread.join()
        
        while not results_queue.empty():
            results.append(results_queue.get())
        
        return results
    
    def _execute_operation(self, operation: Dict[str, Any]) -> Dict[str, Any]:
        start_time = time.time()
        
        try:
            if operation["type"] == "GetItem":
                response = self.table.get_item(Key=operation["key"])
                result = response.get("Item")
            elif operation["type"] == "PutItem":
                response = self.table.put_item(Item=operation["item"])
                result = response
            
            end_time = time.time()
            
            return {
                "success": True,
                "operation": operation["type"],
                "start_time": start_time,
                "end_time": end_time,
                "duration": end_time - start_time,
                "result": result
            }
        except ClientError as e:
            end_time = time.time()
            return {
                "success": False,
                "operation": operation["type"],
                "start_time": start_time,
                "end_time": end_time,
                "duration": end_time - start_time,
                "error": str(e)
            }
    
    def _compile_results(self, results: List[Dict[str, Any]]) -> Dict[str, Any]:
        successful_ops = [r for r in results if r["success"]]
        failed_ops = [r for r in results if not r["success"]]
        
        if not successful_ops:
            return {"error": "No successful operations"}
        
        durations = [r["duration"] for r in successful_ops]
        
        return {
            "total_operations": len(results),
            "successful_operations": len(successful_ops),
            "failed_operations": len(failed_ops),
            "success_rate": len(successful_ops) / len(results),
            "average_duration": sum(durations) / len(durations),
            "min_duration": min(durations),
            "max_duration": max(durations),
            "p50_duration": sorted(durations)[len(durations) // 2],
            "p95_duration": sorted(durations)[int(len(durations) * 0.95)],
            "p99_duration": sorted(durations)[int(len(durations) * 0.99)],
            "operations_per_second": len(successful_ops) / sum(durations)
        }
```

#### Bước 4: Tạo Main Execution Script

```python
# main.py
import yaml
import argparse
import sys
from benchmark_engine import BenchmarkEngine
from mysql_test_runner import MySQLTestRunner
from dynamodb_test_runner import DynamoDBTestRunner

def load_config(config_file: str) -> dict:
    with open(config_file, 'r') as f:
        return yaml.safe_load(f)

def setup_benchmark_engine(config: dict) -> BenchmarkEngine:
    engine = BenchmarkEngine(config)
    
    # Register test runners
    if 'mysql' in config['databases']:
        mysql_runner = MySQLTestRunner(config)
        engine.register_test_runner('mysql', mysql_runner)
    
    if 'dynamodb' in config['databases']:
        dynamodb_runner = DynamoDBTestRunner(config)
        engine.register_test_runner('dynamodb', dynamodb_runner)
    
    return engine

def run_single_test(engine: BenchmarkEngine, test_config: dict):
    print(f"Running benchmark test: {test_config['database_type']} - {test_config['workload']}")
    
    result = engine.run_benchmark(test_config)
    
    print(f"Test completed:")
    print(f"  - Operations per second: {result['test_results'].get('operations_per_second', 'N/A')}")
    print(f"  - Average latency: {result['test_results'].get('average_duration', 'N/A')}ms")
    print(f"  - Success rate: {result['test_results'].get('success_rate', 'N/A')}")
    
    return result

def main():
    parser = argparse.ArgumentParser(description='AWS Database Benchmark Suite')
    parser.add_argument('--config', required=True, help='Configuration file path')
    parser.add_argument('--database', required=True, help='Database type')
    parser.add_argument('--workload', required=True, help='Workload type')
    
    args = parser.parse_args()
    
    # Load configuration
    try:
        config = load_config(args.config)
    except Exception as e:
        print(f"Error loading configuration: {e}")
        sys.exit(1)
    
    # Setup engine
    engine = setup_benchmark_engine(config)
    
    # Create test config
    test_config = {
        'database_type': args.database,
        'workload': args.workload,
        'concurrent_users': 50,
        'duration': 300,
        'warmup_time': 60
    }
    
    # Run test
    result = run_single_test(engine, test_config)
    
    # Save results
    engine.save_results()

if __name__ == '__main__':
    main()
```

#### Bước 5: Tạo Configuration File

```yaml
# benchmark_config.yaml
benchmark:
  name: "AWS Database Benchmark Suite"
  version: "1.0.0"
  description: "Comprehensive database performance testing"

environment:
  region: "ap-southeast-1"
  vpc_id: "vpc-xxxxxxxxx"
  subnet_ids: ["subnet-xxxxxxxxx", "subnet-xxxxxxxxx"]

databases:
  mysql:
    endpoint: "benchmark-mysql.xxxxxxxxx.ap-southeast-1.rds.amazonaws.com"
    port: 3306
    username: "admin"
    password: "YourPassword123!"
    database: "benchmark_db"
  
  dynamodb:
    table_name: "benchmark-table"
    region: "ap-southeast-1"

workloads:
  - name: "OLTP_Standard"
    duration: 300
    concurrent_users: [10, 50, 100, 200]
    warmup_time: 60
  
  - name: "NoSQL_High_Throughput"
    duration: 300
    concurrent_users: [50, 100, 200, 500]
    warmup_time: 60

metrics:
  collection_interval: 5  # seconds
  storage:
    s3_bucket: "benchmark-results-xxxxxxxxx"
    cloudwatch_namespace: "DatabaseBenchmark"

reporting:
  output_format: ["json", "csv", "html"]
  dashboard_url: "https://your-dashboard-url.com"
```

#### Bước 6: Chạy Benchmark Test

```bash
# Install dependencies
pip install mysql-connector-python boto3 pyyaml

# Run MySQL benchmark
python main.py --config benchmark_config.yaml --database mysql --workload OLTP_Standard

# Run DynamoDB benchmark
python main.py --config benchmark_config.yaml --database dynamodb --workload NoSQL_High_Throughput
```

#### Tóm tắt

Trong lab này, bạn đã:
- ✅ Xây dựng core benchmark engine với threading và concurrency
- ✅ Tạo database connectors cho MySQL và DynamoDB
- ✅ Implement automated test execution
- ✅ Tạo configuration system
- ✅ Xây dựng main execution script

**Bước tiếp theo**: [Tạo Comparison Tools và Regression Detection](5.4-comparison-tools/) 