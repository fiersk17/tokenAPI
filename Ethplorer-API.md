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
* [getAddressHistory](#get-last-address-operations)
* [getTokenHistoryGrouped](#get-grouped-token-history)

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

    /getTokenInfo/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=freekey
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
           creatorAddress:  # contract creator address,
           transactionHash: # contract creation transaction hash,
           timestamp:       # contract creation timestamp
        },
        tokenInfo:  # exists if specified address is a token contract address (same format as token info),
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

    /getAddressInfo/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=freekey

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

    /getTxInfo/0x6aa670c983425eba23314459c48ae89b3b8d0e1089397c56400ce2da5ece9d26?apiKey=freekey
***

###Get last token operations

**Request**

    /getTokenHistory/[address]

Additional params

    type:    show operations of specified type only
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

    /getTokenHistory?apiKey=freekey

Show last 5 transfers for token at address 0xff71cb760666ab06aa73f34995b42dd4b85ea07b:

    /getTokenHistory/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=freekey&type=transfer&limit=5
***

###Get last address operations

**Request**

    /getAddressHistory/{address}

Additional params

    token:   show only specified token address operations
    type:    show operations of specified type only
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

Show last MKR token transfers for address 0x1f5006dff7e123d550abc8a4c46792518401fcaf:

    /getAddressHistory/0x1f5006dff7e123d550abc8a4c46792518401fcaf?apiKey=freekey&token=0xc66ea802717bfb9833400264dd12c2bceaa34a6d&type=transfer
***

###Get grouped token history

**Request**

    /getTokenHistoryGrouped/[address]

Additional params

    period:  show operations of specified days number only [optional, 30 days if not set, max. is 90 days]

**Response**

    {
        countTxs: [        # grouped token history
            {
                _id: {
                    year:  # transaction year
                    month: # transaction month
                    day:   # transaction day
                },
                ts:        # timestamp of the first transaction in group
                cnt:       # number of transaction in group
            },
            ...
        ]
    }
**Examples**

Show operations for token at address 0xff71cb760666ab06aa73f34995b42dd4b85ea07b:

    /getTokenHistoryGrouped/0xff71cb760666ab06aa73f34995b42dd4b85ea07b?apiKey=freekey
***

### Error response
    {
        error: {
            code:    # error code (integer),
            message: # error message
        }
    }