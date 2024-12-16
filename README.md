# SAS-SDK

[@jitendra_123/sas-sdk](https://www.npmjs.com/package/@jitendra_123/sas-sdk) is an SDK for interacting with the StarkNet Attestation Service (SAS) on StarkNet. It simplifies schema creation, attestation management, and validation for developers building on StarkNet.

## Features
- **Schema Management**: Create, fetch, and manage schemas.
- **Attestation Services**: Register, fetch, and revoke attestations.
- **Validation**: Verify attestation validity.
- **StarkNet Compatibility**: Built for seamless integration with StarkNet's ZK-rollup ecosystem.

## Installation

Install the SDK via npm:

```bash
npm install @jitendra_123/sas-sdk
```

## Usage

Here is an example of how to use the SAS SDK:

```javascript
const { SAS } = require('@jitendra_123/sas-sdk');
require('dotenv').config();

const {
  ACCOUNT_ADDRESS,
  SR_CONTRACT_ADDRESS,
  SA_CONTRACT_ADDRESS,
  SR_ABI,
  SA_ABI,
} = require('./constants');

(async () => {
  try {
    const sas = new SAS(
      process.env.NODE_URL,
      process.env.PRIVATE_KEY,
      ACCOUNT_ADDRESS,
      SR_CONTRACT_ADDRESS,
      SA_CONTRACT_ADDRESS,
      SR_ABI,
      SA_ABI
    );

    // Register a new schema
    const schema = "felt252 emp_Name, u256 emp_ID, felt252 emp_Teams, u256 emp_BG";
    const resolver = "0x0000000000000000000000000000000000000000";
    const revocable = true;
    const registrationResult = await sas.createSchema(schema, resolver, revocable);
    console.log('Schema registered:', registrationResult);

    // Fetch a schema by UID
    const schemaUID = "0x1956053be88994deebeed5da9405e422a94451e6bc5fb9ff8612b4fa49aece7c";
    const schemaDetails = await sas.getSchema(schemaUID);
    console.log('Fetched Schema:', schemaDetails);

    // Register an attestation
    const recipient = "0x0124f678b5b285a9c88b7283dcd19bf9e2a5f7d89afe0a7cd7ed5da3f3257212";
    const expirationTime = 1764996235;
    const schemaData = "felt252 name Jk, u256 stu_ID 92890, felt252 stu_Branch cse";
    const attestationResult = await sas.registerAttestation(
      recipient,
      expirationTime,
      revocable,
      0,
      schemaData,
      0,
      schemaUID
    );
    console.log('Attestation registered:', attestationResult);

    // Validate attestation
    const attestationUID = "0xaec37c669883d8a6d8db9f9403b707680c1f89fd5167331f0c2651ad14ce82bd";
    const isValid = await sas.isAttestationValid(attestationUID);
    console.log('Is Attestation Valid:', isValid);
  } catch (error) {
    console.error('Error:', error.message);
  }
})();
```

## Environment Variables
Create a `.env` file in your project root and add the following:

```plaintext
NODE_URL=<your-starknet-node-url>
PRIVATE_KEY=<your-private-key>
```

## API Reference

### SAS Class

#### Constructor
```javascript
const sas = new SAS(
  nodeUrl,
  privateKey,
  accountAddress,
  srContractAddress,
  saContractAddress,
  srAbi,
  saAbi
);
```

| Parameter           | Description                                    |
|---------------------|------------------------------------------------|
| `nodeUrl`           | The StarkNet node URL.                        |
| `privateKey`        | Private key for authentication.               |
| `accountAddress`    | StarkNet account address.                     |
| `srContractAddress` | Schema Registry contract address.             |
| `saContractAddress` | Schema Attestation contract address.          |
| `srAbi`             | ABI for the Schema Registry contract.         |
| `saAbi`             | ABI for the Schema Attestation contract.      |

#### Methods

- **`createSchema(schema, resolver, revocable)`**: Registers a new schema.
- **`getSchema(schemaUID)`**: Fetches details of a specific schema.
- **`getAllSchemas()`**: Fetches all registered schemas.
- **`registerAttestation(...)`**: Registers a new attestation.
- **`isAttestationValid(attestationUID)`**: Checks the validity of an attestation.
- **`revokeAttestation(...)`**: Revokes an attestation.

## Repository

For more information, visit the [GitHub repository](https://github.com/jitendragangwar123/SAS-SDK.git).

## License

This SDK is released under the [MIT License](https://opensource.org/licenses/MIT).
