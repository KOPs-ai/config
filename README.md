# Kops AI Configuration Files

This directory contains configuration files for Kops AI's DeFi strategy management system. These files define permission policies and parameter rules for various DeFi protocols and their smart contract interactions.

## File Structure

### `strategy/strategies_usdtYield.json` (Legacy Format)

This file contains the legacy configuration format for USDT yield strategies. It uses a simpler structure with `actionPolicies` arrays.

### `strategy/strategies_usdtYield_v2.json` (Current Format)

This file contains the updated configuration format with enhanced parameter validation and rule-based access control.

## Configuration Structure

Both files contain arrays of strategy objects. Each object represents a specific smart contract function call that is allowed under certain conditions.

### Example Item Structure (from v2 format):

```json
{
  "protocol": "hypurrfi",
  "chainId": "999",
  "fnName": "approve(address,uint256)",
  "actionTargetSelector": "0x095ea7b3",
  "actionTarget": "0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb",
  "policyType": "universal",
  "paramRules": [
    [
      {
        "condition": 0,
        "offset": 0,
        "isLimited": false,
        "ref": "0xcecce0eb9dd2ef7996e01e25dd70e461f918a14b",
        "usage": {
          "limit": 0,
          "used": 0
        }
      },
      {
        "condition": 4,
        "offset": 32,
        "isLimited": false,
        "ref": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "usage": {
          "limit": 0,
          "used": 0
        }
      }
    ]
  ]
}
```

## Field Descriptions

### Top-Level Fields

- **`protocol`** (string): The DeFi protocol name (e.g., "hypurrfi", "hyperlend")
- **`chainId`** (string): The blockchain network identifier (e.g., "999")
- **`fnName`** (string): The smart contract function signature (e.g., "approve(address,uint256)")
- **`actionTargetSelector`** (string): The function selector (first 4 bytes of function signature hash)
- **`actionTarget`** (string): The smart contract address that can be called
- **`policyType`** (string): The type of policy applied (e.g., "universal")

### Parameter Rules (`paramRules`)

The `paramRules` array contains rule sets for validating function parameters. Each rule set is an array of parameter validation rules.

#### Parameter Rule Fields

- **`condition`** (number): The validation condition type
  - `0`: Exact match
  - `1`: Greater than
  - `2`: Less than
  - `3`: Greater than or Equal
  - `4`: Less than or Equal
  - `5`: Not Equal
  - `6`: In range
- **`offset`** (number): The byte offset of the parameter in the function call data
- **`isLimited`** (boolean): Whether this parameter has usage limits
- **`ref`** (string): The reference value for validation
  - Can be a contract address (0x...)
  - Can be a special value like "userAddress" or "orchestratorAddress"
  - Can be a maximum value (0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
- **`usage`** (object): Usage tracking for limited parameters
  - **`limit`** (number): Maximum allowed usage count
  - **`used`** (number): Current usage count

## Supported Protocols

### Hypurrfi

- **Contract**: `0xcecce0eb9dd2ef7996e01e25dd70e461f918a14b`
- **Functions**: approve, supply, withdraw
- **Token**: `0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb`

### Hyperlend

- **Contract**: `0x00a89d7a5a02160f20150ebea7a2b5e4879a1a8b`
- **Functions**: approve, supply, withdraw
- **Token**: `0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb`

## Migration from v1 to v2

The v2 format replaces the simple `actionPolicies` array with a more sophisticated `paramRules` system that provides:

- Granular parameter validation
- Usage tracking and limits
- Multiple validation scenarios per function
- Better security controls

This allows for more precise control over what operations are permitted and under what conditions.
