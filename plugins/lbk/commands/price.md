---
description: Look up an LBK product by SKU, model number or description, with specs and list price
argument-hint: "<SKU, model number or description>"
---

Using the `lbk` connector, look up: $ARGUMENTS

Use `search_products` with what I typed, passing my words as `originalRequest`. If I described a
product without naming a brand, search LBK's go-to brands for that product type first (the
`byProduct` list in `get_my_account`'s `brandPreferences`). Open the best match with
`get_catalog_product` (or `get_product` for an item from LBK's own price list). Show the brand,
name, key specs, the manufacturer list price (called that) and whether LBK carries it. If there's
no list price, say LBK will quote it.
