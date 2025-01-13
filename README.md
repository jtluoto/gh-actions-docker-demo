# GitHub Actions for publishing Docker images

This project is a simple Node.js application that sets up an Express server. It listens on port 3000 and responds with "Hello world" when the root route is accessed.

## Getting Started

To get a copy of the project up and running on your local machine, follow these steps:

### Prerequisites

- Node.js (version 14 or higher)
- npm (Node package manager)

### Installation

1. Clone the repository:
   ```
   git clone <repository-url>
   ```
2. Navigate to the project directory:
   ```
   cd gh-actions-docker-demo
   ```
3. Install the dependencies:
   ```
   npm install
   ```

### Running the Application

To start the server, run the following command:
```
node index.js
```
The server will be running on [http://localhost:3000](http://localhost:3000).

### Docker

To build and run the application using Docker, follow these steps:

1. Build the Docker image:
   ```
   docker build -t gh-actions-docker-demo .
   ```
2. Run the Docker container:
   ```
   docker run -p 3000:3000 gh-actions-docker-demo
   ```
