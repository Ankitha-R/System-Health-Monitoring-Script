pip install psutil

import psutil
import time
import logging

# Setup logging to console and log file
logging.basicConfig(filename='system_health.log',
                    format='%(asctime)s - %(levelname)s - %(message)s',
                    level=logging.INFO)

# Define threshold values
CPU_THRESHOLD = 80  # in percentage
MEMORY_THRESHOLD = 80  # in percentage
DISK_THRESHOLD = 80  # in percentage

# Function to monitor system health
def monitor_system_health():
    # CPU usage
    cpu_usage = psutil.cpu_percent(interval=1)
    if cpu_usage > CPU_THRESHOLD:
        logging.warning(f'High CPU usage detected: {cpu_usage}%')
        print(f'ALERT: High CPU usage: {cpu_usage}%')

    # Memory usage
    memory_info = psutil.virtual_memory()
    memory_usage = memory_info.percent
    if memory_usage > MEMORY_THRESHOLD:
        logging.warning(f'High Memory usage detected: {memory_usage}%')
        print(f'ALERT: High Memory usage: {memory_usage}%')

    # Disk space usage
    disk_info = psutil.disk_usage('/')
    disk_usage = disk_info.percent
    if disk_usage > DISK_THRESHOLD:
        logging.warning(f'High Disk usage detected: {disk_usage}%')
        print(f'ALERT: High Disk usage: {disk_usage}%')

    # Running processes
    process_count = len(psutil.pids())
    logging.info(f'Total running processes: {process_count}')
    print(f'Total running processes: {process_count}')

# Continuous monitoring with a defined interval
def run_monitor(interval=60):
    print("Starting system health monitor...\n")
    while True:
        monitor_system_health()
        time.sleep(interval)

if __name__ == "__main__":
    run_monitor(interval=60)  # Set monitoring interval (in seconds)

