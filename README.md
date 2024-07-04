# Scalable Personality-Based Career Selection Web Application

This repository hosts the source code and resources for the Scalable Personality-Based Career Selection Web Application, developed using full-stack technologies including Next.js, Flask, PostgreSQL, Docker, Kubernetes, EKS AWS, and Locust. This project was created under the guidance of Professor Emiliano Casalicchio by the following team members:

1. **Arman Feili**
   - **Matricola**: 2101835
   - **Email**: feili.2101835@studenti.uniroma1.it
2. **Milad Torabi**
   - **Matricola**: 2103454
   - **Email**: torabi.2103454@studenti.uniroma1.it
3. **Sohrab Seyyedi Parsa**
   - **Matricola**: 2101087
   - **Email**: seyyediparsa.2101087@studenti.uniroma1.it

## Project Description

### Overview

The goal of this project was to develop a scalable web application that analyzes users' personalities using the MBTI and Big Five classifications, leveraging pre-trained models from Hugging Face. Users are asked to complete a series of 60 multiple-choice questions followed by 26 open-ended questions. The responses are analyzed to generate grammatically correct text, which is then used to evaluate the user's personality.

### Personality Analysis

#### MBTI (Myers-Briggs Type Indicator)

The MBTI categorizes individuals into 16 personality types based on four dichotomies:
- **Introversion (I) vs. Extraversion (E)**
- **Sensing (S) vs. Intuition (N)**
- **Thinking (T) vs. Feeling (F)**
- **Judging (J) vs. Perceiving (P)**

We utilized the "JanSt/albert-base-v2_mbti-classification" model from Hugging Face for MBTI analysis.

#### Big Five Personality Traits

The Big Five assesses personality based on five dimensions:
- **Openness** (inventive/curious vs. consistent/cautious)
- **Conscientiousness** (efficient/organized vs. easy-going/careless)
- **Extraversion** (outgoing/energetic vs. solitary/reserved)
- **Agreeableness** (friendly/compassionate vs. challenging/detached)
- **Neuroticism** (sensitive/nervous vs. resilient/confident)

We used the "Minej/bert-base-personality" model from Hugging Face for Big Five analysis.

### Features

- Displays MBTI personality types as a pie chart.
- Shows Big Five personality traits as a bar chart.
- Provides career suggestions based on personality types, ranked by average salary in the United States.
- Stores user data, including responses, MBTI and Big Five results, and career suggestions in a PostgreSQL database.

### Technical Implementation

#### Frontend

- **Next.js**: Used for server-side rendering and static site generation.

#### Backend

- **Flask**: Serves as the backend framework.
- **PostgreSQL**: Manages data storage.

#### Containerization

- **Docker**: Containerizes the frontend, backend, and database for consistent environments.

#### Orchestration

- **Kubernetes**: Manages and orchestrates Docker containers.
- **EKS AWS**: Elastic Kubernetes Service used for scalable and secure deployment.

#### Load Testing

- **Locust**: Simulates user activity to test scalability and performance.

### Deployment

The application is live and accessible at the following URL: [Web Application](http://ad0b4043cf0c74a9d99733004f9ad7bd-66423231.us-east-1.elb.amazonaws.com/)
The Locust testing interface is available at: [Locust Interface](http://a97b3a6e06cec478b87ac8e494c4b948-978139299.us-east-1.elb.amazonaws.com:8089/)

### Project Structure

```
.
├── Kubernetes
│   ├── db-deployment.yaml
│   ├── flask-deployment.yaml
│   ├── locust-configmap.yaml
│   ├── locust-deployment.yaml
│   ├── next-deployment.yaml
│   ├── persistent-volume-claim.yaml
│   └── persistent-volume.yaml
├── backend
│   ├── api.py
│   ├── app.py
│   ├── datasets
│   ├── migrations
│   ├── models.py
│   ├── requirements.txt
│   ├── flask.dockerfile
│   ├── compose.yml
│   ├── alembic.ini
│   ├── env.py
│   ├── script.py.mako
│   └── versions
├── frontend
│   ├── components
│   ├── pages
│   ├── public
│   ├── styles
│   ├── next-env.d.ts
│   ├── next.config.js
│   ├── next.dockerfile
│   ├── package-lock.json
│   ├── package.json
│   ├── postcss.config.js
│   ├── tailwind.config.ts
│   ├── tsconfig.json
│   └── README.md
├── testing
│   ├── locustfile.py
│   └── questionsAndOptions.json
├── 5-big-traits-career.xlsx
└── MBTI-career.xlsx
```

### Issues and Challenges

1. **AWS Student Account Limitations**: Faced limitations on creating users for EKS, leading to unexpected costs.
2. **Integrating Multiple Frameworks**: Ensured seamless operation across Next.js, Flask, PostgreSQL, Docker, Kubernetes, EKS AWS, and Locust.
3. **AWS Configuration**: Required detailed attention to AWS best practices and thorough testing.
4. **Optimizing YAML Configurations**: Updated YAML files for optimal server configurations and resource allocation.
5. **Datasets for Career Recommendations**: Involved extensive research and data collection for accurate recommendations.
6. **Generating Test Data**: Created a JSON file with randomized answers for comprehensive testing.
7. **Managing Dependencies**: Ensured compatibility between numerous Python and JavaScript packages.

### Commands and Configuration

#### AWS CLI for EKS User

```bash
aws configure --profile eks-user
```

#### Create EKS Cluster

```bash
eksctl create cluster --name eks-cluster-1 --region us-east-1 --nodes 3 --node-type m5.xlarge --profile eks-user
```

#### Configure `kubectl`

```bash
aws eks --region us-east-1 update-kubeconfig --name eks-cluster-1 --profile eks-user
```

#### Deploy Persistent Volumes

```bash
kubectl apply -f Kubernetes/persistent-volume.yaml
kubectl apply -f Kubernetes/persistent-volume-claim.yaml
```

#### Deploy Applications

```bash
kubectl apply -f Kubernetes/db-deployment.yaml
kubectl apply -f Kubernetes/flask-deployment.yaml
kubectl apply -f Kubernetes/next-deployment.yaml
```

#### Verify Deployments

```bash
kubectl get pods -n my-app
kubectl get svc -n my-app
```

### Environment Variables

#### Backend Configuration (`/backend/.env`)

```plaintext
DATABASE_URL=postgresql://postgres:postgres@db-service:5432/postgres
```

#### Frontend Configuration (`/frontend/.env.local`)

```plaintext
NEXT_PUBLIC_API_URL=http://a11043d4ebd004509aa9a413be69311b-689262011.us-east-1.elb.amazonaws.com:4000
```

### Connectivity Test

```bash
wget -qO- http://a11043d4ebd004509aa9a413be69311b-689262011.us-east-1.elb.amazonaws.com:4000/health
```

### Results and Analysis

- **Scalability**: Handled up to 700 users smoothly; performance degraded beyond this point due to insufficient resources.
- **Performance**: Identified the need for stronger instances like `m5.2xlarge` for better handling increased loads.

For detailed performance metrics and further analysis, please refer to the project documentation.

---

This project demonstrates the successful integration of modern web technologies to create a robust, scalable personality analysis and career recommendation web application.