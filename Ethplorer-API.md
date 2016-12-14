Ethplorer’s API may be used to get information about Ethereum tokens, contracts, transactions and custom structures. This is still beta version of service. There is no any warranty for provided data.

Free API-Key is `freekey`. Please don’t overload our servers. We would much appreaciate “Powered by [Ethplorer.io](https://ethplorer.io)” backlink on your pages.
If you need more data or highload of service – [contact us](mailto:support@ethplorer.io) to get personal API key. 

# API Index
Api's server address: https://api.ethplorer.io/, method GET.
Each request should have a mandatory apiKey parameter.

## Methods

* [getTokenInfo](#get-token-info)
* [getAddressInfo](#get-address-info)
* [getTxInfo](#get-transaction-info)
* [getTokenHistory](#get-last-token-operations)

## Errors

* [Error response](#error-response)

***

### Get token info

**Request**

    /getTokenInfo/{address}

**Response**

    {
        address:     # token address,
        totalSupply: # total token supply,
        name:        # token name,
        symbol:      # token symbol,
        decimals:    # number of significant digits,
        owner:       # token owner address,
        countOps:    # total count of token operations
    }

**Examples**

Request:

    /getTokenInfo/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=xxxxxxx
Response:

    {
        address:     "0xff71cb760666ab06aa73f34995b42dd4b85ea07b",
        totalSupply: "1000000000",
        name:        "THBEX",
        symbol:      "THBEX",
        decimals:    "4",
        owner:       "0x6016dca5eb73590fa875fcf32bdb74905a4323bd",
        countOps:    320
    }
***

###Get address info

**Request**

    /getAddressInfo/{address}
Additional params

    token: show balances for specified token address only

**Response**

    {
        address: # address,
        ETH: {   # ETH specific information
            balance:  # ETH balance
            totalIn:  # Total incoming ETH value
            totalOut: # Total outgoing ETH value
        },
        contractInfo: {  # exists if specified address is a contract
            code:        # contract code,
            created: {   # contract creation information
               creatorAddress:  # contract creator address,
               transactionHash: # contract creation transaction hash,
               timestamp:       # contract creation timestamp
            }
        },
        tokens: [   # exists if specified address has any token balances
            {
                tokenInfo: # token data (same format as token info),
                balance:   # token balance (as is, not reduced to a floating point value),
                totalIn:   # total incoming token value
                totalOut:  # total outgoing token value
            },
            ...
        ],
        countTxs:    # Total count of incoming and outcoming transactions (including creation one),
    }

**Examples**

Request:

    /getAddressInfo/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=xxxxxxx

***

###Get transaction info

**Request**

    /getTxInfo/{transaction hash}

***Response***

    {
        hash:          # transaction hash,
        timestamp:     # transaction block time,
        blockNumber:   # transaction block number,
        confirmations: # number of confirmations,
        success:       # true if there were no errors during tx execution
        from:          # source address,
        to:            # destination address,
        value:         # ETH send value,
        input:         # transaction input data (hex),
        gasLimit:      # gas limit set to this transaction,
        gasUsed:       # gas used for this transaction,
        logs: [        # event logs
            {
                address: # log record address
                topics:  # log record topics
                data:    # log record data
            },
            ...
        ],
        operations: [  # token operations list for this transaction
            {
                # Same format as /getTokenHistory
            },
            ...
        ]
    }

**Examples**

Request:

    /getTxInfo/0x6aa670c983425eba23314459c48ae89b3b8d0e1089397c56400ce2da5ece9d26?apiKey=xxxxxxx
***

###Get last token operations

**Request**

    /getTokenHistory/[address]

Additional params

    type:    show operations of specified type only
    tsAfter: show operations with timestamp greater than this value
    limit:   maximum number of operations [1 - 10, default = 10]

**Response**

    {
        operations: [
            {
                timestamp:       # operation timestamp
                transactionHash: # transaction hash
                tokenInfo:       # token data (same format as token info),
                type:            # operation type (transfer, approve, issuance, mint, burn, etc),
                address:         # operation target address (if one),
                from:            # source address (if two addresses involved),
                to:              # destination address (if two addresses involved),
                value:           # operation value (as is, not reduced to a floating point value),
            },
            ...
        ]
    }
**Examples**

Show last 10 token operations:

    /getTokenHistory?apiKey=xxxxxxx

Show last 5 transfers for token at address 0xff71cb760666ab06aa73f34995b42dd4b85ea07b:

    /getTokenHistory/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=xxxxxxx&type=transfer&limit=5
***

### Error response
    {
        error: {
            code:    # error code (integer),
            message: # error message
        }
    }