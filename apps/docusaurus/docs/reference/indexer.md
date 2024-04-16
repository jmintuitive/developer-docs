---
title: "Indexer API Reference"
---

# TODOs BEFORE MERGING
Decisions:
1. For tables which have nested data structures, how deep should this page go? (Do we stop at the first level? Case-by-case basis?)
    - For now, I've only included the 1st level of data. I'm worried things will be too cluttered otherwise, but it'd be much better reference material if so. - Jackson

Work to do:
1. Add public-facing descriptions for each table.
2. Add the type information and descriptions to "good to use" tables.
3. Provide alternatives for deprecated tables if there are any.
4. If there is a better grouping for these tables, update that.
5. (Optional) Provide an example use case for the table to the description.
6. (Optional) Change the date on the deprecation notice. (I guessed for it, assumed all would have the same date, and applied the notice both to the "deprecation" and the "private" fields - so please correct me where I'm wrong!)

Don't worry about the formatting on whitespace for "Description" and such - I'll run a script afterwards before we merge to fix the whitespacing - Jackson

# Indexer API Reference

The Indexer API allows you to access rich data about tokens, accounts, transactions, and events on-chain using GraphQL queries.

You can interactively explore the data discussed here using Hasura explorers here:
- Mainnet: https://cloud.hasura.io/public/graphiql?endpoint=https://api.mainnet.aptoslabs.com/v1/graphql
- Testnet: https://cloud.hasura.io/public/graphiql?endpoint=https://api.testnet.aptoslabs.com/v1/graphql
- Devnet: https://cloud.hasura.io/public/graphiql?endpoint=https://api.devnet.aptoslabs.com/v1/graphql

Or via code at the following endpoints:
- Mainnet: https://api.mainnet.aptoslabs.com/v1/graphql
- Testnet: https://api.testnet.aptoslabs.com/v1/graphql
- Devnet: https://api.devnet.aptoslabs.com/v1/graphql

As you explore, you can refer back to these reference docs to understand the tables you are interested in. You should use Ctrl + F to quickly find the right entry for that table.

At the bottom of this page we list the tables which are deprecated, along with any alternatives which may replace them. Make sure to check before building any production infrastructure that the table you are using is not deprecated as there was recently a v2 which dramatically simplified the list of tables.

:::tip
If you are looking up a table with the `_by_pk` suffix, search for the table name without that suffix. `_by_pk` tables are automatically generated for convenience to allow querying by primary key.
:::

# Indexer Table Reference
:::tip
Remember to use Ctrl + F to find the table you are interested in!
:::

## General

### `account_transactions`
_Has an aggregate view for summary data called `account_transactions_aggregate`_

mapping between accounts to transactions that touch that account

| Field                        | Type    | Description |
|------------------------------|---------|-------------|
| account_address              | String! |             |
| coin_activities              | Table   |             |
| coin_activities_aggregate    | Table   |             |
| delegated_staking_activities | Table   |             |
| fungible_asset_activities    | Table   |             |
| token_activities             | Table   |             |
| token_activities_aggregate   | Table   |             |
| token_activities_v2          | Table   |             |
| token_activities_v2_aggregate| Table   |             |
| transaction_version          | bigint! |             |

### `ledger_infos`
which chain, should be largely irrelevant

| Field         | Type   | Description |
|---------------|--------|-------------|
| chain_id      | String |             |

### `processor_status`
gives you latest version processed per “processor”

| Field                     | Type   | Description |
|---------------------------|--------|-------------|
| last_success_version      | bigint |             |
| last_transaction_timestamp| bigint |             |
| last_updated              | bigint |             |
| processor                 | String |             |

## NFT

### `token_activities_v2`
_Has an aggregate view for summary data called `token_activities_v2_aggregate`_

NFT activities, include both v1 and v2

| Field                     | Type   | Description |
|---------------------------|--------|-------------|
| aptos_names_owner         | Table  |             |
| aptos_names_owner_aggregate| Table |             |
| aptos_names_to            | Table  |             |
| aptos_names_to_aggregate  | Table  |             |
| coin_amount               | bigint |             |
| coin_type                 | String |             |
| collection_data_id_hash   | String |             |
| collection_name           | String |             |
| creator_address           | String |             |
| current_token_data        | Table  |             |
| event_account_address     | String |             |
| event_creation_number     | bigint |             |
| event_index               | bigint |             |
| event_sequence_number     | bigint |             |
| from_address              | String |             |
| name                      | String |             |
| property_version          | bigint |             |
| to_address                | String |             |
| token_amount              | bigint |             |
| token_data_id_hash        | String |             |
| transaction_timestamp     | bigint |             |
| transaction_version       | bigint |             |
| transfer_type             | String |             |

### `nft_metadata_crawler_parsed_asset_uris`
cdn of the images for nfts

| Field                          | Type   | Description |
|--------------------------------|--------|-------------|
| animation_optimizer_retry_count| Int    |             |
| asset_uri                      | String |             |
| cdn_animation_uri              | String |             |
| cdn_image_uri                  | String |             |
| cdn_json_uri                   | String |             |
| image_optimizer_retry_count    | Int    |             |
| json_parser_retry_count        | Int    |             |
| raw_animation_uri              | String |             |
| raw_image_uri                  | String |             |

### `current_token_ownerships_v2`
_Has an aggregate view for summary data called `current_token_ownerships_v2_aggregate`_

who owns which nft, includes token v1. Note, fungible tokens are exceptions. They are currently in all the token tables but we might want to remove them.

| Field                        | Type   | Description |
|------------------------------|--------|-------------|
| amount                       | bigint |             |
| aptos_name                   | String |             |
| collection_data_id_hash      | String |             |
| collection_name              | String |             |
| creator_address              | String |             |
| current_collection_data      | Table  |             |
| current_token_data           | Table  |             |
| last_transaction_timestamp   | bigint |             |
| last_transaction_version     | bigint |             |
| name                         | String |             |
| owner_address                | String |             |
| property_version             | bigint |             |
| table_type                   | String |             |
| token_data_id_hash           | String |             |
| token_properties             | Table  |             |

### `current_token_datas_v2`
metadata of each nft, including uri, etc, includes token v1.

| Field                           | Type   | Description |
|---------------------------------|--------|-------------|
| aptos_name                      | String |             |
| cdn_asset_uris                  | Table  |             |
| collection_id                   | bigint |             |
| current_collection              | Table  |             |
| current_token_ownerships        | Table  |             |
| current_token_ownerships_aggregate | Table  |         |
| decimals                        | bigint |             |
| description                     | String |             |
| is_fungible_v2                  | Boolean|             |
| largest_property_version_v1     | bigint |             |
| last_transaction_timestamp      | bigint |             |
| last_transaction_version        | bigint |             |
| maximum                         | bigint |             |
| supply                          | bigint |             |
| token_data_id                   | bigint |             |
| token_name                      | String |             |
| token_properties                | Table  |             |
| token_standard                  | String |             |
| token_uri                       | String |             |

### `current_collections_v2`
metadata for each nft collection, includes token v1

| Field                        | Type | Description |
|------------------------------|--------|-------------|
| cdn_asset_uris               | Table  |             |
| collection_id                | bigint |             |
| collection_name              | String |             |
| creator_address              | String |             |
| current_supply               | bigint |             |
| description                  | String |             |
| last_transaction_timestamp   | bigint |             |
| last_transaction_version     | bigint |             |
| max_supply                   | bigint |             |
| mutable_description          | String |             |
| mutable_uri                  | String |             |
| table_handle_v1              | String |             |
| token_standard               | String |             |
| total_minted_v2              | bigint |             |
| uri                          | String |             |

### `current_collection_ownership_v2_view`
_Has an aggregate view for summary data called `current_collection_ownership_v2_view_aggregate`_

a view that groups owners and collection, and provides the count of distinct nfts owned

| Field                        | Type   | Description |
|------------------------------|--------|-------------|
| collection_id                | bigint |             |
| collection_name              | String |             |
| collection_uri               | String |             |
| creator_address              | String |             |
| current_collection           | Table  |             |
| distinct_tokens              | bigint |             |
| last_transaction_version     | bigint |             |
| owner_address                | String |             |
| single_token_uri             | String |             |

## Fungible Assets

### `fungible_asset_metadata`
documentation for each fungible asset, e.g. decimals, supply, etc, includes coin v1

| Field                                | Type   | Description |
|--------------------------------------|--------|-------------|
| asset_type                           | String |             |
| creator_address                      | String |             |
| decimals                             | bigint |             |
| icon_uri                             | String |             |
| last_transaction_timestamp           | bigint |             |
| last_transaction_version             | bigint |             |
| name                                 | String |             |
| project_uri                          | String |             |
| supply_aggregator_table_handle_v1    | String |             |
| supply_aggregator_table_key_v1       | String |             |
| symbol                               | String |             |
| token_standard                       | String |             |

### `fungible_asset_activities`
fungible asset activities, includes coin v1

| Field                           | Type   | Description |
|---------------------------------|--------|-------------|
| amount                          | bigint |             |
| asset_type                      | String |             |
| block_height                    | bigint |             |
| entry_function_id_str           | String |             |
| event_index                     | bigint |             |
| gas_fee_payer_address           | String |             |
| is_frozen                       | Boolean|             |
| is_gas_fee                      | Boolean|             |
| is_transaction_success          | Boolean|             |
| metadata                        | Table  |             |
| owner_address                   | String |             |
| owner_aptos_names               | Table  |             |
| owner_aptos_names_aggregate     | Table  |             |
| storage_id                      | String |             |
| storage_refund_amount           | bigint |             |
| token_standard                  | String |             |
| transaction_timestamp           | bigint |             |
| transaction_version             | bigint |             |
| type                            | String |             |

### `current_fungible_asset_balances`
_Has an aggregate view for summary data called `current_fungible_asset_balances_aggregate`_

fungible asset balances per owner, includes coin v1

| Field                       | Type   | Description |
|-----------------------------|--------|-------------|
| amount                      | bigint |             |
| asset_type                  | String |             |
| is_frozen                   | Boolean|             |
| is_primary                  | Boolean|             |
| last_transaction_timestamp  | bigint |             |
| last_transaction_version    | bigint |             |
| metadata                    | Table  |             |
| owner_address               | String |             |
| storage_id                  | String |             |
| token_standard              | String |             |

## Aptos Naming Service (ANS)

### `current_aptos_names`
_Has an aggregate view for summary data called `current_aptos_names_aggregate`_

view created based on current_ans_lookup_v2 for easy query

| Field                       | Type   | Description |
|-----------------------------|--------|-------------|
| domain                      | String |             |
| domain_with_suffix          | String |             |
| expiration_timestamp        | bigint |             |
| is_active                   | Boolean|             |
| is_domain_owner             | Boolean|             |
| is_primary                  | Boolean|             |
| last_transaction_version    | bigint |             |
| owner_address               | String |             |
| registered_address          | String |             |
| subdomain                   | String |             |
| token_name                  | String |             |
| token_standard              | String |             |

### `current_ans_lookup_v2`
raw table for current_aptos_names

| Field                     | Type   | Description |
|---------------------------|--------|-------------|
| domain                    | String |             |
| expiration_timestamp      | bigint |             |
| is_deleted                | Boolean|             |
| last_transaction_version  | bigint |             |
| registered_address        | String |             |
| subdomain                 | String |             |
| token_name                | String |             |
| token_standard            | String |             |

# Deprecated Tables

The following tables are planned for deprecation, or are already deprecated. See the notes section for any direct replacements or notes on how to migrate if you currently depend on one of these tables. Please do not use any of the below tables for production services.

| Table                                                 | Deprecation Date | Notes |
|-------------------------------------------------------|------------------|-------|
| address_events_summary                                | May 15th         |       |
| address_version_from_events                           | May 15th         |       |
| address_version_from_events_aggregate                 | May 15th         |       |
| address_version_from_move_resources                   | May 15th         |       |
| address_version_from_move_resources_aggregate         | May 15th         |       |
| block_metadata_transactions                           | May 15th         |       |
| coin_activities                                       | May 15th         |       |
| coin_activities_aggregate                             | May 15th         |       |
| coin_balances                                         | May 15th         |       |
| coin_infos                                            | May 15th         |       |
| coin_supply                                           | May 15th         |       |
| collection_datas                                      | May 15th         |       |
| current_ans_lookup                                    | May 15th         |       |
| current_coin_balances                                 | May 15th         |       |
| current_collection_datas                              | May 15th         |       |
| current_delegated_staking_pool_balances               | May 15th         |       |
| current_delegated_voter                               | May 15th         |       |
| current_delegator_balances                            | May 15th         |       |
| current_objects                                       | May 15th         |       |
| current_staking_pool_voter                            | May 15th         |       |
| current_table_items                                   | May 15th         |       |
| current_token_datas                                   | May 15th         |       |
| current_token_ownerships                              | May 15th         |       |
| current_token_ownerships_aggregate                    | May 15th         |       |
| current_token_pending_claims                          | May 15th         |       |
| delegated_staking_activities                          | May 15th         |       |
| delegated_staking_pool_balances                       | May 15th         |       |
| delegated_staking_pool_balances_aggregate             | May 15th         |       |
| delegated_staking_pools                               | May 15th         |       |
| delegator_distinct_pool                               | May 15th         |       |
| delegator_distinct_pool_aggregate                     | May 15th         |       |
| events                                                | May 15th         |       |
| move_resources                                        | May 15th         |       |
| move_resources_aggregate                              | May 15th         |       |
| nft_marketplace_v2_current_nft_marketplace_auctions   | May 15th         |       |
| nft_marketplace_v2_current_nft_marketplace_collection_offers | May 15th         |       |
| nft_marketplace_v2_current_nft_marketplace_listings   | May 15th         |       |
| nft_marketplace_v2_current_nft_marketplace_listings_aggregate | May 15th         |       |
| nft_marketplace_v2_current_nft_marketplace_token_offers | May 15th         |       |
| nft_marketplace_v2_nft_marketplace_activities         | May 15th         |       |
| num_active_delegator_per_pool                         | May 15th         |       |
| proposal_votes                                        | May 15th         |       |
| proposal_votes_aggregate                              | May 15th         |       |
| signatures                                            | May 15th         |       |
| table_items                                           | May 15th         |       |
| table_metadatas                                       | May 15th         |       |
| token_activities                                      | May 15th         |       |
| token_activities_aggregate                            | May 15th         |       |
| token_activities_v2                                   | May 15th         |       |
| token_activities_v2_aggregate                         | May 15th         |       |
| token_datas                                           | May 15th         |       |
| token_ownerships                                      | May 15th         |       |
| tokens                                                | May 15th         |       |
| transaction_version                                   | May 15th         |       |
| user_transactions                                     | May 15th         |       |
