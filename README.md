# Secure Key Management System

This project implements a Secure Key Management System that handles both symmetric and asymmetric encryption. It includes secure key generation and storage, a key exchange mechanism using Diffie-Hellman, and a key revocation system.

## Features
- **Symmetric Key Generation**: Generates and securely stores a 256-bit AES key.
- **Asymmetric Key Generation**: Creates RSA key pairs (2048-bit) and stores them securely.
- **Secure Key Exchange**: Uses Diffie-Hellman to securely generate a shared key.
- **Key Revocation**: Implements a mechanism to revoke compromised keys.

## Technologies Used
- Python
- PyCryptodome
- Google Colab (for development)

## Installation
1. Install dependencies:
   ```bash
   pip install pycryptodome
   ```
2. Clone the repository:
   ```bash
   git clone <repository-url>
   cd secure-key-management
   ```
3. Run the script in Google Colab or a local Python environment.

## Code Explanation
### 1. Secure Key Generation and Storage
#### Symmetric Key Generation
```python
def generate_symmetric_key():
    key = get_random_bytes(32)  # 256-bit AES key
    with open("symmetric_key.bin", "wb") as f:
        f.write(key)
    return key
```
This function generates a 256-bit AES symmetric key and saves it to a binary file (`symmetric_key.bin`).

#### Asymmetric Key Generation
```python
def generate_asymmetric_keys():
    key = RSA.generate(2048)
    private_key = key.export_key()
    public_key = key.publickey().export_key()
    with open("private.pem", "wb") as f:
        f.write(private_key)
    with open("public.pem", "wb") as f:
        f.write(public_key)
    return private_key, public_key
```
This function generates a 2048-bit RSA key pair (private and public keys) and saves them in separate `.pem` files.

### 2. Secure Key Exchange Using Diffie-Hellman
```python
def diffie_hellman_key_exchange():
    p = 23  # Prime number
    g = 5   # Generator

    private_key_a = int.from_bytes(get_random_bytes(16), "big") % p
    private_key_b = int.from_bytes(get_random_bytes(16), "big") % p

    public_key_a = (g ** private_key_a) % p
    public_key_b = (g ** private_key_b) % p

    shared_secret_a = (public_key_b ** private_key_a) % p
    shared_secret_b = (public_key_a ** private_key_b) % p

    assert shared_secret_a == shared_secret_b  # Both should compute the same shared secret

    shared_key = hashlib.sha256(str(shared_secret_a).encode()).digest()
    return shared_key
```
This function simulates the Diffie-Hellman key exchange to generate a shared secret key securely.

### 3. Key Revocation Mechanism
#### Revoke a Key
```python
revoked_keys = set()

def revoke_key(key_hash):
    revoked_keys.add(key_hash)
```
This function adds a key hash to the `revoked_keys` set, marking it as revoked.

#### Check if a Key is Revoked
```python
def is_key_revoked(key):
    key_hash = hashlib.sha256(key).hexdigest()
    return key_hash in revoked_keys
```
This function checks if a given key is revoked by computing its hash and comparing it with the stored revoked keys.

### 4. Running the Functions
```python
aes_key = generate_symmetric_key()
print("Symmetric Key Generated and Stored.")

private_key, public_key = generate_asymmetric_keys()
print("Asymmetric Keys Generated and Stored.")

shared_key = diffie_hellman_key_exchange()
print("Diffie-Hellman Key Exchange Successful.")

# Simulating key revocation
revoke_key(hashlib.sha256(aes_key).hexdigest())
print("AES Key Revoked.")

if is_key_revoked(aes_key):
    print("AES Key is Revoked.")
else:
    print("AES Key is Valid.")
```
These function calls execute the key management system's operations, including key generation, exchange, and revocation.

## Google Colab Notebook
You can run this project in Google Colab by opening the following link:
### https://colab.research.google.com/github/manishashetty29/Key_Management_System/blob/main/Key_Management_System.ipynb


## Contributing
1. Fork the repository.
2. Create a new branch (`feature-branch`).
3. Commit your changes.
4. Push to the branch.
5. Open a Pull Request.



