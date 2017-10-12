# WholeTale & DataONE Integration

## Executive Summary

- I'm going to provide an easy way from within [search.dataone.org](https://search.dataone.org) to regsiter datasets into WholeTale
- Until DataONE and WholeTale auth are integrated, the easiest way to do this is by redirecting the user from [search.dataone.org](https://search.dataone.org) to WholeTale with the dataset(s) to be registered set up as query parameters
- This, while easy for me, will require changes in the **Dashboard** to recognize the redirect request
- Users will be able to register one or more datasets at a time

## Background

WholeTale is all about analyzing data so making data available to a running WholeTale front-end should be easy for the user.
There are currently two approaches to get data into WholeTale:

1. From within a running front-end (e.g. RStudio Server), by retrieving data over the Internet with code running in the front-end
2. Register a dataset before launching the front-end

While approach (1) works and fits well within the reproducibility goals of WholeTale, (2) is arguably more formal and, unlike (1), can make use of the WholeTale backend infrastructure to make the registered dataset available to the running front-end which is more efficient.

Data registration currently happens during the set-up phase along with selecting a front-end image and can register data from a limited set of providers.

TODO: One right now?

For registering data from DataONE, we have implemented a simple DataONE search interface that requires the user to specify a DataONE identiifer which is just about the most difficult way we can come up with for a user to register DataONE data with WholeTale.
Because DataONE already has a feature-rich [search tool](https://search.dataone.org) (DataONE Search), it makes a lot of sense to also allow registration of data from within DataONE Search.

While searching the **Catalog**, a list of datasets matching the user's set of filters is shown:

![DataONE Search](./images/dataone_search.png)

and when a user clicks on one of those datasets, a **Landing Page** is shown:

![DataONE Landing Page](./images/dataone_landing_page.png)

## Proposal

I propose DataONE search provides a way for users to register DataONE data into WholeTale in both the **Catalog** and **Landing Page** views.

TODO: Modalities

- Register the current dataset or currently selected datasets to WholeTale
- Add the current dataset or currently selected datasets to a "Shopping Cart", which then you'd have to click another button to Register with WholeTale

TODO: Method of registration

- If API, we need to support WT (Globus) auth from within DataOE
- If redirect, we can rely on existing infrastructure

### Implementation details

How this is implemented makes a huge difference in the difficulty of implementation.
The current API already already has a method for registering a dataset, `POST /dataset/register`, which requires a payload like:

```json
{
    "dataId": "urn:uuid:42969280-e11c-41a9-92dc-33964bf785c8",
    "doi": "10.5063/F1Z899CZ",
    "name": "Data from a dynamically downscaled projection of past and future microclimates covering North America from 1980-1999 and 2080-2099",
    "repository": "DataONE",
    "size": 178679
}
```

This is (or should be) an authenticated route and, at the present, DataONE authentication knows nothing about WholeTale authentication (*Note: this may change*). 
Therefore, this existing route would is not workable for now.

A way around this is to simply redirect the user to WholeTale to a URL on the **Dashboard** that users query parameters to tell WholeTale which dataset(s) to register, e.g.,

`https://search.dataone.org/#view/doi:10.18739/A2JB90 -> https://dashboard.wholetale.org/register/dataone/doi%3A10.18739%2FA2JB90`

**Upsides:**

- No need (*for now*) to integrate DataONE and WholeTale authentication
- User automatically appears in WholeTale, ready to work

**Downsides:**

- Various clients have limitations on the length or URLs so the user would only be able to register a finite number of datasets at a time

### User flow

How the user navigates through this matters.

### Catalog integration

### Landing page integration


### Decision points

- Should we do the redirect approach given the downsides?
- Yes/no on shopping cart idea
- Verbage / button appearance
