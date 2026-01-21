# pyoqs-wasm

Pyodide-compatible ctypes interface for Open Quantum Safe (liboqs). This is a refactored liboqs-python wheel designed specifically for WebAssembly environments.

## Installation

### For Pyodide/PyScript

```python
import micropip
await micropip.install("pyoqs-wasm")
```

### For standard Python

```bash
pip install pyoqs-wasm
```

## Usage

```python
import oqs

# List available KEM algorithms
print(oqs.get_enabled_kem_mechanisms())

# Key Encapsulation
with oqs.KeyEncapsulation("ML-KEM-768") as kem:
    public_key = kem.generate_keypair()
    ciphertext, shared_secret_enc = kem.encap_secret(public_key)
    shared_secret_dec = kem.decap_secret(ciphertext)

# Digital Signatures  
with oqs.Signature("ML-DSA-65") as signer:
    public_key = signer.generate_keypair()
    signature = signer.sign(b"Message to sign")
    is_valid = signer.verify(b"Message to sign", signature, public_key)
```

## Differences from liboqs-python

- Bundles a WebAssembly-compiled `liboqs.so` for use in Pyodide
- Loads `liboqs.so` from the package directory when running under Pyodide
- Relies on upstream ctypes behavior (no extra `argtypes` overrides)
- Keeps the upstream auto-install fallback on non-Pyodide environments

## Versioning

The version follows the upstream liboqs-python `X.Y.Z` scheme. Patch
releases may add a trailing `.P` when needed.

## Credits

Based on [liboqs-python](https://github.com/open-quantum-safe/liboqs-python) by the Open Quantum Safe project.

## License

MIT License (same as original liboqs-python). Third-party notices for the
bundled liboqs binary are in `THIRD_PARTY_LICENSES.md`.
