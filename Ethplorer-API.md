Ethplorer’s API may be used to get information about Ethereum tokens, contracts, transactions and custom structures. This is still beta version of service. There is no any warranty for provided data.

Free API-Key is “freekey”. Please don’t overload our servers. We would much appreaciate “Powered by ethplorer.io” backlink on your pages.
If you need more data or high load of service – contact us to get personal API key. 

# API Description
Api's server address: https://api.ethplorer.io/, method POST.
Each request should have a mandatory apiKey parameter.

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

###Get address info

**Request**

    /getAddressInfo/{address}

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

**Additional params (POST)**

    token: show balances for specified token address only
