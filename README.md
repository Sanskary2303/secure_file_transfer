# **Secure File Transfer**

**Secure File Transfer** is a Python library designed for secure, reliable, and efficient file transfers. This library provides a protocol that supports features like encryption, file compression, checksum verification, resumption of interrupted transfers, and the prioritization of multiple files. It uses SSL/TLS to ensure secure communication between the client and server.

---

## **Features**

- **Secure Communication**: SSL/TLS encryption for secure file transfer.
- **File Compression**: Compresses files before transfer to save bandwidth.
- **Checksum Validation**: Ensures data integrity with SHA-256 checksum verification.
- **Resumption Support**: Resumes transfers from the last byte in case of interruptions.
- **File Prioritization**: Supports prioritizing files for transfer based on their importance.
- **Parallel Transfers**: Enables concurrent file transfers for efficiency.

---

## **Installation**

You can install the package directly using pip:

```bash
pip install secure_file_transfer
```

## **Quickstart**

### Server Setup

To start the server, import the `SecureFileTransferServer` class, initialize it with the required parameters, and call the `start()` method:

```python
from secure_file_transfer import SecureFileTransferServer

server = SecureFileTransferServer(
    host="127.0.0.1", 
    port=12345, 
    certfile="server_cert.pem", 
    keyfile="server_key.pem"
)
server.start()
```

### Client Setup

To send files using the client, import the `SecureFileTransferClient` class, initialize it with the required parameters, and call the `send_file()` or  `send_files_in_parallel()` method:

```python
from secure_file_transfer import SecureFileTransferClient

client = SecureFileTransferClient(
    host="127.0.0.1", 
    port=12345, 
    cafile="ca_cert.pem"
)

# Send a single file
client.send_file("example.txt")

# Send multiple files in parallel
client.send_files_in_parallel(["file1.txt", "file2.txt", "file3.txt"])
```

## **Features in Detail**
1. **Secure Communication**
   - The protocol uses SSL/TLS to encrypt communication between the client and server.
   - Requires a valid server certificate (`server_cert.pem`) and a trusted CA certificate (`ca_cert.pem`).

2. **File Compression**

    - Files are compressed using gzip before transfer to reduce bandwidth usage.
    Compressed files are automatically cleaned up after the transfer is complete.

3. **Checksum Validation**

    - The client computes an SHA-256 checksum of the file and sends it to the server.
    - The server verifies the checksum after receiving the file to ensure data integrity.

4. **Resumption Support**

    - The server checks for partially received files and notifies the client to resume from the last byte.

5. **File Prioritization**

    - Files can be prioritized, and higher-priority files are transferred first.
    Implemented using a priority queue on the server.

6. **Parallel Transfers**

   -  Multiple files can be transferred concurrently using the `send_files_in_parallel()` method.

## **Certificate Setup**

To use SSL/TLS, generate the required certificates:

1. **Create a root CA certificate:**

```bash
openssl req -x509 -newkey rsa:4096 -keyout ca_key.pem -out ca_cert.pem -days 365 -nodes
```

2. **Create a server certificate:**

```bash
openssl req -new -newkey rsa:4096 -keyout server_key.pem -out server_csr.pem -nodes
openssl x509 -req -in server_csr.pem -CA ca_cert.pem -CAkey ca_key.pem -CAcreateserial -out server_cert.pem -days 365
```

Place these files in the appropriate directories and reference them in your code.

## **Teating**

### **Simulate File Transfers**

- Start the server:

```bash
python server.py
```
- Run the client to transfer files:

```bash
python client.py
```

**Simulate Network Interruptions**

- Interrupt a transfer and restart the client to test resumption.

**Simulate Multiple Clients**

- Run the client script on multiple machines or in parallel.

## **Customization**

### **Adjust Compression Levels**

You can modify the compression level (1–9) when calling `compress_file()` in the client.

### **Dynamic Chunk Sizes**

To dynamically adjust chunk sizes, implement a negotiation mechanism between the client and server.