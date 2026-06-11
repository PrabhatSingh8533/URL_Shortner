# ShortURL

A highly scalable, full-stack URL shortening service designed to efficiently convert long URLs into compact, easy-to-share links. The architecture is built for high availability and high performance, utilizing a modern tech stack and microservices-inspired deployment via Docker.

## 🚀 Features

- **URL Shortening**: Easily convert any long URL into a short, manageable link.
- **High Availability**: Utilizes NGINX as a reverse proxy and load balancer to distribute incoming traffic across multiple Node.js backend server instances.
- **Fast Retrieval**: Integrates Redis for caching frequently accessed URLs, ensuring blazing-fast redirects.
- **Distributed Coordination**: Leverages Apache Zookeeper for robust distributed coordination, such as managing range allocations for unique alias generation across multiple server instances.
- **Persistent Storage**: MongoDB is used for reliable, persistent storage of URLs and their associated short mappings.
- **Client-Side Optimization**: The React frontend uses Bloom Filters to quickly verify if a custom alias is potentially available without making unnecessary backend roundtrips.
- **Containerized Deployment**: Fully containerized using Docker and Docker Compose for seamless setup, scaling, and deployment.

## 🛠️ Technology Stack

**Frontend (Client)**
- React (Create React App)
- React Router DOM
- SCSS for styling
- Axios for API requests
- Bloom Filters

**Backend (Server)**
- Node.js & Express.js
- Mongoose (MongoDB ODM)
- Redis Client
- Node Zookeeper Client
- Cron (for background tasks like purging expired aliases)

**Infrastructure & DevOps**
- Docker & Docker Compose
- NGINX (Load Balancer)
- Redis (Caching)
- Apache Zookeeper (Distributed System Coordination)
- MongoDB (Database)

## 🏗️ Architecture

1. **Client**: A React application running on port `3000`.
2. **NGINX**: Acts as an API gateway/load balancer running on port `4000`, routing traffic to the backend instances.
3. **Backend Servers**: Multiple instances of the Node.js Express server running internally on port `8081`. 
4. **Zookeeper**: Coordinates the backend servers for unique token/alias generation.
5. **Redis**: Caches URL mappings to reduce database load.
6. **MongoDB**: The primary database storing the URL mappings.

## 🏁 Getting Started

### Prerequisites

Make sure you have the following installed on your machine:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Installation & Running

1. **Clone the repository:**
   ```bash
   git clone <your-repository-url>
   cd ShortURL-master
   ```

2. **Start the application with Docker Compose:**
   The `docker-compose.yml` file is configured to build the images. The configuration also allows you to scale the Node.js backend (e.g., to 3 instances) to demonstrate load balancing:
   ```bash
   docker-compose up --build --scale node-server=3
   ```

3. **Access the Application:**
   - **Frontend UI**: Open your browser and navigate to `http://localhost:3000`
   - **Backend API (via NGINX Load Balancer)**: `http://localhost:4000`

### API Endpoints

- `POST /url` - Create a new shortened URL.
- `GET /url/:identifier` - Retrieve the original URL for a given identifier.
- `GET /del` - Delete/purge an identifier token.

## 📝 Background Workers

The backend includes cron-based background workers (e.g., `purgeAliases`) designed to periodically clean up expired or unused aliases from the database and cache, keeping the system optimized.
