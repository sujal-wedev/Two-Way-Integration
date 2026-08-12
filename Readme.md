# Two-Way Integration App

This FastAPI-based application facilitates two-way integration between your product's customer catalog and external systems like Stripe. It uses Docker containers for MySQL and Kafka and provides a seamless data synchronization mechanism.

## Getting Started

Follow these steps to set up and run the application on your local system:

1. Clone this repository:
    ```shell
    git clone https://github.com/sujal-wedev/zenskar.git
    ```

2. Create a .env file in the root directory of the project and fill in the required environment variables:
    ```shell
    DB_HOST=<database-host>
    DB_ROOT_PASSWORD=<root-password>
    DB_DATABASE=<database-name>
    DB_USER=<database-user>
    DB_PASSWORD=<database-password>
    DB_PORT=<database-port>
    KAFKA_TO_STRIPE_TOPIC=<kafka-topic>
    STRIPE_SECRET_KEY=<stripe-secret-key>
    STRIPE_WEBHOOK_SIGNING_KEY=<stripe-webhook-signing-key>
    NGROK_SECRET_KEY=<ngrok-secret-key>
    ```
- DB_HOST: Hostname or IP address of the database server.
- DB_ROOT_PASSWORD: Root user password for the database.
- DB_DATABASE: Name of the database.
- DB_USER: Username for the database.
- DB_PASSWORD: Password for the database user.
- DB_PORT: Port on which the database is running.
- KAFKA_TO_STRIPE_TOPIC: Kafka topic for communication with Stripe.
- STRIPE_SECRET_KEY: Stripe secret key obtained from your Stripe account.
- STRIPE_WEBHOOK_SIGNING_KEY: Signing key for Stripe webhook.
- NGROK_SECRET_KEY: Ngrok secret key obtained from the Ngrok dashboard.

Note: DB_HOST should be set to localhost, and you can choose any non-root value for DB_USER. Create accounts on ngrok and stripe. Create a fake webhook on stripe and copy the signing key. Get access to stripe secret key at [STRIPE SECRET KEY](https://dashboard.stripe.com/test/apikeys) and to ngrok secret key at [NGROK SECRET KEY](https://dashboard.ngrok.com/get-started/your-authtoken).

3.  Start the Docker containers using the following command:
    ```shell
    docker-compose up --build
    ```

4.  Once the containers are running, navigate to the app directory and install the required Python packages:
    ```shell
    cd app
    pip install -r requirements.txt
    ```

5.  Start the FastAPi application:
    ```shell
    python app.py
    ```

6. After the app starts, go to the [Ngrok dashboard](https://dashboard.ngrok.com/cloud-edge/endpoints), copy the Ngrok URL, and update the fake Stripe webhook with [ngrok-endpoint]/webhook/stripe.

7. You are now ready to add or delete customers from both Stripe and your application. Use the following API endpoints:
    - To add a customer using the API, send a POST request to http://localhost:5000/customer with JSON containing the customer's email and name.
    - To delete a customer using the API, send a DELETE request to http://localhost:5000/customer/[:id].
    - To view all customers, send a GET request to http://localhost:5000/customers.





## Future Plans

### Salesforce Integration

The following lays down plan to integrate with Salesforce.

#### Implementation

- **Directory Structure**: In our project, we will create a new directory within integrations named salesforce. Inside this directory, we will have two main files:
  - salesforce_integrations.py: This file will contain the necessary SDK functions to interact with Salesforce, such as creating and deleting customer records.
  - salesforce_webhooks.py: Similar to the Stripe integration, this file will handle incoming webhooks from Salesforce. These webhooks will trigger corresponding database functions to reflect changes in Salesforce within the application.

#### Usage

- **API Integration**: When specific APIs are called within the  application (e.g., creating a customer), will trigger calls to the Salesforce SDK to ensure that customer data is also synchronized with Salesforce.

- **Webhook Handling**: The Salesforce webhooks will interact with the database functions to make corresponding changes in the database when Salesforce data changes. This ensures that the customer catalog remains consistent.

### Extending Customer Catalog to Invoice Catalog

The following lays down plan on how to extend to customer catalog, to maybe invoice catalog, or others.

#### Implementation

- **Database Functions**: Will introduce additional database functions designed to handle changes in the invoice catalog. These functions will facilitate the creation, modification, and deletion of invoices.

- **API and Webhook Utilization**: The same set of database functions can be utilized through both FastAPI  and webhooks to make necessary changes in the database. For example, creating an invoice can be triggered via API, which will then interact with the relevant database function to create the invoice record.



