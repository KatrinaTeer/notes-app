\# Lab 5 Evidence



\## Part 1 – Docker and Docker Compose



The notes application was containerized using a Dockerfile and deployed locally with Docker Compose. The Compose environment included the web application and a PostgreSQL database with persistent storage.



Testing confirmed:

\- The web application started successfully.

\- The `/readyz` endpoint returned `{"status":"ready"}`.

\- Notes could be created and retrieved through the API.

\- PostgreSQL data persisted after the containers were stopped and restarted.

\- Docker layer caching reused unchanged dependency layers during application-only rebuilds.

\- Changing `requirements.txt` caused the dependency installation layer to rebuild.



\## Part 2 – CI/CD and Container Registry



GitHub Actions was configured to automatically build and publish the application image to Docker Hub.



Docker Hub repository:



`katrinateer/notes-app`



The workflow publishes:

\- `latest`

\- Commit-specific SHA tags



The image is built for both:

\- `linux/amd64`

\- `linux/arm64`



A successful GitHub Actions run and the multi-platform Docker Hub image tags were verified.



\## Part 3 – Kubernetes



A local Kubernetes cluster named `lab5` was created using kind.



The application was deployed in the `notes-lab` namespace with:

\- One PostgreSQL Deployment

\- One PostgreSQL Service

\- One PostgreSQL PersistentVolumeClaim

\- Two web application replicas

\- One web Service

\- A ConfigMap for database configuration

\- A Kubernetes Secret for the database password

\- A readiness probe using `/readyz`



`kubectl get all,pvc -n notes-lab` confirmed:

\- PostgreSQL pod: `1/1 Running`

\- Web Deployment: `2/2` replicas available

\- PostgreSQL and web Services created

\- `postgres-pvc`: `Bound`, 1 GiB



\## Persistence Test



A note containing `hello from kubernetes` was created through the application.



The PostgreSQL pod was then deliberately deleted. Kubernetes automatically created a replacement database pod.



After the replacement pod became ready, the note was retrieved successfully:



`"body":"hello from kubernetes"`



This demonstrated both Kubernetes self-healing and persistence of PostgreSQL data through the PersistentVolumeClaim.



\## Service and Replica Test



Requests were sent to the web Service from inside the Kubernetes cluster.



Responses were returned by both web replicas, including:



`web-6f794d8cb8-tsjnw`



and



`web-6f794d8cb8-9cspw`



This demonstrated that the Kubernetes Service routed requests to multiple web application replicas.



\## Rollout History



The web Deployment rollout history showed revision 1:



`REVISION 1`



\## Repository



GitHub repository:



`https://github.com/KatrinaTeer/notes-app`

