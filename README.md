
import socket
import time

server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server.bind(("0.0.0.0", 1234))

print("Time Server Running...")

while True:
    data, addr = server.recvfrom(1024)
    message = data.decode()

    if message.lower() == "stop":
        server.sendto("Server stopped".encode(), addr)
        break

    elif message.lower() == "time":
        current_time = time.ctime()
        server.sendto(current_time.encode(), addr)

    else:
        server.sendto("Invalid request".encode(), addr)

server.close()





import socket

client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

server_address = ("127.0.0.1", 1234)

while True:
    message = input("Enter request (time/stop): ")

    client.sendto(message.encode(), server_address)

    data, addr = client.recvfrom(1024)

    print("Server:", data.decode())

    if message.lower() == "stop":
        break

client.close()
