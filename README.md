# SpringAI Milvus Vector Demo

This project demonstrates the integration of Milvus, a vector database, with Spring Boot to build a vector store service. The application provides endpoints for loading documents into the Milvus vector store and searching for similar documents using vector embeddings.

## Prerequisites

Before running the project, ensure you have the following installed:

- **Java 21**: Required to build and run the Spring Boot application.
- **Maven**: Used to build the project.
- **Docker**: Required to run Milvus in a container.
- **Milvus Vector Database**: Milvus is used to store and search vector data. You can install Milvus via Docker.

## Setting Up Milvus with Docker

Milvus provides a powerful vector database that you can easily set up using Docker. Follow the steps to install Milvus on Google Cloud Platform (GCP) by following the guide here:

[Milvus Standalone Installation (Docker)](https://milvus.io/docs/install_standalone-docker.md)

### Quick Docker Command to Install Milvus

You can run Milvus using the following Docker command:

```bash
docker pull milvusdb/milvus:v2.2.7
docker run -d --name milvus-standalone \
 -p 19530:19530 \
 -p 9091:9091 \
 milvusdb/milvus:v2.2.7
```

### Running Milvus on GCP

To set up Milvus on GCP, use the provided Docker setup commands above after configuring your GCP instance. Refer to the [Milvus installation guide](https://milvus.io/docs/install_standalone-docker.md) for detailed instructions on how to install Milvus in your cloud environment.

## Project Setup

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/LegPro/springai-milvus-vector-demo.git
   cd springai-milvus-vector-demo
   ```

2. **Set Up Milvus on GCP or Local:**
   Follow the Docker installation steps above to install Milvus.

3. **Configure Milvus in Application:**
   In the `application.yml` file, configure your Milvus vector store:

   ```yaml
   spring:
     ai:
       vectorstore:
         milvus:
           client:
             host: ${gcp.milvus} # Ensure this is the correct IP address of your Milvus instance
             port: 19530
             username: "root"
             password: "milvus"
           databaseName: "default"
           collectionName: "vector_store"
           embeddingDimension: 1536
           indexType: IVF_FLAT
           metricType: COSINE
   ```

4. **Run the Application:**

   You can run the Spring Boot application using Maven:

   ```bash
   mvn spring-boot:run
   ```

5. **Endpoints:**

    - **Load Documents**: Load vectorized documents into the Milvus store.
      ```bash
      http://localhost:8080/load
      ```

    - **Search Documents**: Search for similar documents based on a vector query.
      ```bash
      http://localhost:8080/search?query=Technology
      ```

## OpenAI Key (Embedding Service)

This project uses OpenAI embeddings for vector generation. Ensure that you have an OpenAI API key and have set it as an environment variable:

```bash
export OPENAI_API_KEY=<your-openai-api-key>
```

You can configure this API key in your environment and the application will use it to generate vector embeddings for the documents.

## Docker Setup for Milvus on GCP

If you're setting up Milvus on GCP, make sure that the instance hosting Milvus is correctly configured to allow network traffic on the necessary ports (19530 for Milvus).

Follow the steps provided in the official Milvus documentation: [Milvus Standalone Installation](https://milvus.io/docs/install_standalone-docker.md).
