# Cloud Pricing API

This project aims to create a GraphQL cloud pricing API. Currently only AWS resources are supported, but future support for other cloud vendors is planned.

This is an early stage project, pull requests to add resources/fix bugs are welcome.

## Table of Contents

* [Example requests](#example-requests)
* [Getting Started](#getting-started)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
* [Usage](#usage)
  * [Running](#running)
* [Future work](#future-work)
* [Contributing](#contributing)
* [License](#license)

## Example requests

Get all t3.micro prices in US East, this returns 30+ results. Try it yourself by pasting the query into [https://pricing.infracost.io/graphql](https://pricing.infracost.io/graphql).

```graphql
query {
  products(
    filter: {
      vendorName: "aws",
      service: "AmazonEC2",
      productFamily: "Compute Instance",
      region: "us-east-1",
      attributeFilters: [
        { key: "instanceType", value: "t3.micro" }
      ]
    },
  ) {
    attributes { key, value }
    prices { USD }
  }
}
```

Get the hourly on-demand price of a Linux EC2 t3.micro instance in US East:

Request:

```graphql
query {
  products(
    filter: {
      vendorName: "aws",
      service: "AmazonEC2",
      productFamily: "Compute Instance",
      region: "us-east-1",
      attributeFilters: [
        { key: "instanceType", value: "t3.micro" },
        { key: "tenancy", value: "Shared" },
        { key: "operatingSystem", value: "Linux" },
        { key: "capacityStatus", value: "Used" },
        { key: "preInstalledSw", value: "NA" }
      ]
    },
  ) {
    prices(
      filter: {
        purchaseOption: "on_demand"
      },
    ) { USD }
  }
}
```

Response:

```json
{
  "products": [
    {
      "pricing": [
        {
          "USD": "0.0104000000"
        }
      ]
    }
  ]
}
```

## Getting started

### Prerequisites

 * Node.js >= 12.18.0
 * MongoDB >= 3.6

### Installation

1. Clone the repo

  ```sh
  git clone https://github.com/infracost/cloud-pricing-api.git
  cd cloud-pricing-api
  ```

2. Add a `.env` file to point to your MongoDB server, e.g.

  ```
  MONGODB_URI=mongodb://localhost:27017/cloudPricing
  ```

3. Install the npm packages

  ```sh
  npm install
  ```

4. Update the pricing data
   **Note: this downloads about 1.8 GB of data**

  ```sh
  npm run update
  ```

## Usage

### Running

```
npm start
```

You can now access the GraphQL Playground at [http://localhost:4000/graphql](http://localhost:4000/graphql).

## Future work

 * Additional vendors
 * A more user-friendly API - this will require adding mappings for all AWS services.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License

[Apache License 2.0](https://choosealicense.com/licenses/apache-2.0/)
