# About Daml Finance

**This writeup is a place holder which will be rewritten after the entire course content is built up**


## Why Daml Finance
Daml Finance supports the modeling of financial and non-financial use cases in Daml. It provides a standard way to represent assets on Daml ledgers and defines common behaviors and rules. There are two main benefits to using the library in your application:

### Shortened time-to-market

Implementing basic financial concepts like ownership or economic terms of an asset is a complex and tedious task. By providing common building blocks, Daml Finance increases delivery velocity and shortens the time-to-market when building Daml applications. The rich set of functionality of Daml Finance is at your disposal so you don’t have to reinvent the wheel.

### Application composability

Building your application on Daml Finance makes it compatible with other platforms in the wider ecosystem. By using a shared library assets become “mobile”, allowing them to be used seamlessly across application boundaries without the need for translation or integration layers. For instance, a Daml Finance-based asset that is originated in a bond issuance application can be used in the context of a secondary market trading application that is also built on Daml Finance.

## What is Daml Finance
Daml Finance is a library that covers the following areas:

- Holdings: modeling of ownership structures, custodial relationships, intermediated securities, and accounts

- Instruments: structuring the economic terms of an asset and the events that govern its evolution

- Settlement: executing complex transactions involving multiple parties and assets

- Lifecycling: governing the evolution of financial instruments over their lifetime


It optimizes for the following aspects:

### Accessibility

The library is designed to have a low barrier to entry. Users familiar with Daml can get started quickly and leverage the provided functionality easily.

### Maintainability

Building with Daml Finance decouples your application code from the underlying representation of assets. This allows the application to evolve without the need to migrate assets from one version to another, and makes maintenance easier.

### Extensibility

Various extension points allow customization and extension of the library as required. If an existing implementation does not fulfil the requirements it is straightforward to provide a custom extension.
