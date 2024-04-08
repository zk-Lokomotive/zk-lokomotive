# zk-lokomotive (Zk Based Fully Secure and Trustless Multichain File Transfer System)
Author: [Baturalp Güvenç](https://github.com/virjilakrum)


![logo(3) 1](https://github.com/zk-Lokomotive/zk-lokomotive/assets/158029357/a5504168-988c-464f-bf15-e6a650f46586)

## Team 

![lokomotive](https://github.com/zk-Lokomotive/zk-lokomotive/assets/158029357/90ba0ad9-97ad-4546-a31c-7df99ca0a426)



## Introduction

* This project aims to create a secure and efficient file transfer system bridging Solana and Ethereum networks by integrating ZK, IPFS, and Wormhole. ZK ensures file integrity and privacy, IPFS facilitates file storage, and Wormhole enables cross-chain token and data transfer.

This project encompasses file transfer scenarios within the **Solana-Solana** and **Solana-EVM** ecosystems. 

---

In the Solana-Solana file transfer scenario, WebRTC is employed instead of Wormhole, with the endpoints (IDs) being the users' wallet addresses on the Solana network. Within the Solana network, the file transfer process operates as follows: RSA keys are utilized to encrypt AES keys, facilitating their secure exchange between the two clients. Subsequently, the file is encrypted using the exchanged AES key and decrypted accordingly by the recipient.

---

On the other hand, the architecture undergoes a significant shift for file transfer between Solana and EVM-based networks. In this scenario, I establish a bridge between Solana and EVM networks. The file is encrypted with zero-knowledge proofs (ZK) and transformed into a token in exchange for tokens. This token, encapsulating the ZK proof of the file's integrity, traverses the Solana-EVM bridge via Wormhole. Upon reception, the recipient validates the ZK proof to claim the file. Additionally, I retrieve the file from IPFS, ensuring its integrity based on the verified proof.

In summary, this project implements a sophisticated file transfer mechanism, leveraging different technologies for seamless communication between Solana-Solana and Solana-EVM networks. Through meticulous encryption, tokenization, and validation processes, it ensures the secure and verifiable exchange of files while maintaining compatibility and interoperability across disparate blockchain ecosystems.

## Motivation:

Providing the amount of our data continues rapidly in the technological singularity. Traditional file formats often lack adequate privacy and security, especially when it comes to sensitive data. File services run on a central server, creating the risk of data breaches and privacy violations. We offer a different solution to this than traditional breaks, by combining the powerful capacities and polynomials of mathematics and the decentralized generality of Solana, thus challenging the trilemma.

## Solution:

Adopting a decentralized approach to file transfer, utilizing the principles of zero-knowledge proofs (zkSNARKs), can significantly enhance privacy and security. This method allows for the verification of file transmission without revealing the contents of the files to the network or a third party.

## Scenario Secure Research File Sharing

### Actors

    Dr. Z: A biomedical researcher working at a prestigious university.
    Mr. B: A collaborator working in a separate research lab at the same university.
    "Solana(mainnet) & Ethereum" Foundation: Funding Dr. Z's research and awaiting updates on progress.

### Problem

Dr. Z is conducting very sensitive and experimental research on a new cancer treatment. He is asking for input from collaborating scientists (like Mr. B) before he is sure of his results. But leaking data can have serious consequences because of the intellectual property of the research and its implications for potential patent applications. In addition, the accuracy and validity of the research data needs to be proven.

### Solution: (Solana-EVM) Integration with ZK File Transfer

![Architecture](https://github.com/zk-Lokomotive/zk-lokomotive/assets/158029357/e2fa9fa0-6bde-4433-82b5-3713dac536e4)


### Benefits:

    Privacy: ZK encryption guarantees that sensitive data can only be accessed by Dr. Johnson and authorized collaborators.
    Integrity: ZK proofs confirm that research data has not been altered in a secure environment.
    Traceability: Solana and Ethereum blockchains provide a permanent record of transactions.
    Trust: The protocol uses both cryptographic techniques and blockchain technology to share research data in a trusted way.
    Wormhole Integration: Seamless communication between Solana and Ethereum allows collaborators and stakeholders to be on different blockchains.

### Enhancements

    Access Control: Access permission efficiency can be increased with advanced smart contracts in Solana.
    User Interface: An easy-to-use web or mobile interface to follow the entire process.
    Data Analytics: Analytics operations on data stored in Ethereum & Solana.

## Used Technologies:

### Wormhole Integration

```rust


pub fn transfer_sol_to_eth(
    ctx: CpiContext<'_, '_, '_, '_, MultichainTransfer>,
    receiver_address: Pubkey,
    amount: u64,
) -> Result<()> {
    let solana_wallet = SolanaWallet::new(ctx.accounts.connection.clone());
    bridge.transfer(
        &solana_wallet,
        &Token::native_sol(),
        amount,
        "ethereum",
        receiver_address.as_ref(),
    )?;

    Ok(())
}
```

```rust

pub struct ZkFile {
    pub content: Vec<u8>, // Encrypted file content using ZK
    pub proof: Vec<u8>, // ZK proof
}

impl ZkFile {
    pub fn from_bytes(file_content: &[u8]) -> Self {
        let (content, proof) = zk_proof::encrypt(file_content);

        ZkFile {
            content,
            proof,
        }
    }

    pub fn to_data(&self) -> ZkFileData {
        ZkFileData {
            content: self.content.clone(),
            proof: self.proof.clone(),
            ..Default::default()
        }
    }
}
```

- The provided Rust struct ZkFile and its methods demonstrate the usage of ZK to encrypt and prove files' integrity. from_bytes function encrypts a file content using ZK, while to_data function converts a ZkFile object to ZkFileData format.

```rust

use ipfs_api::IpfsClient;

fn upload_file(client: &IpfsClient, file_content: &[u8]) -> Result<String> {
    let mut add_result = client.add(file_content)?;
    Ok(add_result.hash.to_string())
}
```

- The provided Rust function upload_file demonstrates the usage of IPFS-http-client to interact with IPFS. It uploads file content to IPFS and returns the IPFS hash.

### Connection Overview

    File content is encrypted using ZKFile, generating a ZK proof.
    Encrypted file is uploaded to IPFS, obtaining an IPFS hash.
    ZkFile object, IPFS hash, and other metadata are converted to ZKFileData format.
    ZKFileData object is transferred to the recipient using Wormhole bridge.

# Architecture Definition:

<img width="1066" alt="diagram-1" src="https://github.com/zk-Lokomotive/zk-lokomotive/assets/158029357/489427f8-d4fa-48d9-93a0-91bec10d5846">

ZK File Transfer is a secure and private method for transferring files between two parties.


## Technical Implementation::

The ZK File Transfer, process uses advanced cryptographic techniques to ensure that files are transferred securely and privately:

1. **WebRTC Configuration:** Utilize the RTCPeerConnection API to configure the WebRTC connection. Include STUN/TURN servers in the configuration for NAT traversal.

2. **Encryption and Decryption:** Use RSA keys for encryption and decryption of AES keys. Implement the exchange of AES keys between clients securely.


```
git clone https://github.com/briansmith/crypto-bench && cd crypto-bench && cargo update && cargo +nightly bench
```

You must use Rust Nightly because `cargo bench` is used for these benchmarks,
and only Right Nightly supports `cargo bench`.

You don't need to run `cargo build`, and in fact `cargo build` does not do
anything useful for this crate.

`./cargo +nightly test` runs one iteration of every benchmark for every
implementation. This is useful for quickly making sure that a change to the
benchmarks does not break them. Do this before submitting a pull request.

`cargo update` in the workspace root will update all the libraries to the
latest version.

`cargo +nightly bench -p crypto_bench_ring` runs all the tests for [_ring_](https://github.com/briansmith/ring).
| | _ring_ | rust-crypto | rust-nettle (Nettle) | rust-openssl (OpenSSL) | sodiumoxide (libsodium) | Windows CNG | Mac/iOS Common Crypto |
|----------------------------------------------|:------------------:|:------------------:|----------------------|:----------------------:|:-----------------------:|:-----------:|:---------------------:|
| ECDH (Suite B) key exchange | :white_check_mark: | | | | | | |

```
// -----------------------------------------------------------------------------
// Eliptic Curve Diffie-Hellman [ECDH] Key Exchange Protocol
// -----------------------------------------------------------------------------
static void
crecip(felem out, const felem z) {
  felem a,t0,b,c;

  /* 2 */ fsquare_times(a, z, 1); // a = 2
  /* 8 */ fsquare_times(t0, a, 2);
  /* 9 */ fmul(b, t0, z); // b = 9
  /* 11 */ fmul(a, b, a); // a = 11
  /* 22 */ fsquare_times(t0, a, 1);
  /* 2^5 - 2^0 = 31 */ fmul(b, t0, b);
  /* 2^10 - 2^5 */ fsquare_times(t0, b, 5);
  /* 2^10 - 2^0 */ fmul(b, t0, b);
  /* 2^20 - 2^10 */ fsquare_times(t0, b, 10);
  /* 2^20 - 2^0 */ fmul(c, t0, b);
  /* 2^40 - 2^20 */ fsquare_times(t0, c, 20);
  /* 2^40 - 2^0 */ fmul(t0, t0, c);
  /* 2^50 - 2^10 */ fsquare_times(t0, t0, 10);
  /* 2^50 - 2^0 */ fmul(b, t0, b);
  /* 2^100 - 2^50 */ fsquare_times(t0, b, 50);
  /* 2^100 - 2^0 */ fmul(c, t0, b);
  /* 2^200 - 2^100 */ fsquare_times(t0, c, 100);
  /* 2^200 - 2^0 */ fmul(t0, t0, c);
  /* 2^250 - 2^50 */ fsquare_times(t0, t0, 50);
  /* 2^250 - 2^0 */ fmul(t0, t0, b);
  /* 2^255 - 2^5 */ fsquare_times(t0, t0, 5);
  /* 2^255 - 21 */ fmul(out, t0, a);
}
```

![An-example-of-ECC-version-of-Diffie-Hellman-Protocol](https://github.com/virjilakrum/zk-lokomotive/assets/158029357/338121bb-a462-40b1-a561-034dd9010c4f)

3. **Zero-Knowledge Proofs for File Integrity:** To verify that a file has been transmitted without revealing its content, zero-knowledge proofs are utilized. These proofs allow the sender to prove that the file matches a publicly agreed-upon hash without disclosing the file itself.

```
echo 'prepare phase1'
node ../../../snarkjs/build/cli.cjs powersoftau new bn128 12 pot12_0000.ptau -v

echo 'contribute phase1 first'
node ../../../snarkjs/build/cli.cjs powersoftau contribute pot12_0000.ptau pot12_0001.ptau --name="First contribution" -v -e="random text"

echo 'apply a random beacon'
node ../../../snarkjs/build/cli.cjs powersoftau beacon pot12_0001.ptau pot12_beacon.ptau 0102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f 10 -n="Final Beacon"

echo 'prepare phase2'
node ../../../snarkjs/build/cli.cjs powersoftau prepare phase2 pot12_beacon.ptau pot12_final.ptau -v

echo 'Verify the final ptau'
node ../../../snarkjs/build/cli.cjs powersoftau verify pot12_final.ptau
```

4. **Poseidon Hash for Data Integrity:** The integrity and authenticity of the file are ensured using the Poseidon hash function, a cryptographic hash function optimized for zero-knowledge proofs. This function is applied to the file before transmission, creating a digest that can be securely compared by the recipient.

![20-Table6-1](https://github.com/virjilakrum/zk-lokomotive/assets/158029357/6f57ca6d-f074-4106-aed1-067ab9277003)

4. **Decentralized Verification:** The verification process, including the checking of zero-knowledge proofs, is performed on a blockchain network (e.g., Solana). This decentralized approach eliminates the need for a trusted third party, enhancing the security and privacy of the file transfer.

5. **Efficient Data Storage on Blockchain:** To maintain efficiency and minimize blockchain storage requirements, only essential data (e.g., proofs, hashes) are stored on-chain. The actual file remains with the sender and recipient, ensuring privacy.

6. **Server:** The system operates on a peer-to-peer basis, with each participant running the ZK File Transfer client. This design supports direct, secure file transfers without intermediaries.

https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API

https://github.com/virjilakrum/zk-lokomotive

- WebRTC (Web Real-Time Communication) is a technology that enables Web applications and sites to capture and optionally stream audio and/or video media, as well as to exchange arbitrary data between browsers without requiring an intermediary. The set of standards that comprise WebRTC makes it possible to share data and perform teleconferencing peer-to-peer, without requiring that the user install plug-ins or any other third-party software.

      WebRTC Configuration: Use the RTCPeerConnection API to configure the WebRTC connection. Include STUN/TURN servers in the configuration to handle NAT traversal.
      Data Channels: Use RTCDataChannel for transferring the encrypted AES key, zk-SNARK proof, and IPFS hash. This channel can also be used for the actual file transfer if not using IPFS.
      Signaling Implementation: Implement a simple signaling mechanism using WebSockets or any real-time communication library. This is for exchanging WebRTC offer, answer, and ICE candidates.

  https://microsoft.github.io/MixedReality-WebRTC/versions/release/1.0/manual/helloworld-unity-signaler.html

8. **Client Interface:** The user interface for ZK File Transfer is designed to be intuitive, allowing users to easily send and receive files securely. The cryptographic operations are handled in the background, providing a seamless experience for the user.

9. **Wallet Connection** The "Wallet Connection" button facilitates a secure linkage between users and their digital wallets, designed specifically for compatibility with the Solana blockchain using @solana/web3js library. Leveraging the advanced capabilities of the Phantom Wallet, this integration enables efficient management of digital assets and seamless file transactions with a person.

https://solana-labs.github.io/solana-web3.js/

    The function transfer_sol_to_eth in lib.rs is used to transfer assets from Solana to Ethereum.
    The bridge and solana_wallet variables are used to create a Wormhole bridge and Solana wallet.
    The wormhole_sdk::transfer function is used to initiate the asset transfer.
    The transfer_sol_to_eth function in main.rs is used for testing.
    The solana_provider variable is used to create Anchor's Solana provider.
    The ix variable is used to create a command to interact with the smart contract.
    The function solana_provider.send_and_confirm is used to send the transaction to the Solana network.

    __
    *First, run for the frontend development server:*
    __
    ```bash
    npm run dev
    # or
    yarn dev
    # or
    pnpm dev
    # or
    bun dev
    ```

    Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

**
_Anchor Solana Cli Build_
**

`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

`rustc --version`

`cargo install --git https://github.com/coral-xyz/anchor anchor-cli --locked`

`anchor --version`

`sh -c "$(curl -sSfL https://release.solana.com/v1.18.4/install)"`

`solana --version`

`cd tokenswap_contract`

`anchor build`

`anchor test`

![Thank You](https://github.com/zk-Lokomotive/zk-lokomotive/assets/158029357/af24913b-b649-4bf5-90e1-fdd99e68ca51)


