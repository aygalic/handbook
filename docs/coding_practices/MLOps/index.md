# MLOps and Machine Learning Engineering

## Machine Learning Pipeline Components

### Version Control for ML
* **Code Version Control**
    - Git best practices for ML projects
    - Branching strategies (feature branches, model versions)
    - `.gitignore` for ML projects (data, models, logs)

* **Data Version Control**
    - DVC (Data Version Control)
    ```python
    # DVC example
    dvc init
    dvc add data/training_data.csv
    dvc remote add -d storage s3://mybucket/dvcstore
    dvc push
    ```
    - Benefits:
        - Track large files
        - Reproduce experiments
        - Share data between team members

* **Model Version Control**
    - MLflow model registry
    - Model versioning strategies
    - Metadata tracking

### Experiment Tracking
* Components to track:
    - Model parameters
    - Training metrics
    - Dataset versions
    - Environment details

* **MLflow Example**
```python
import mlflow

with mlflow.start_run():
    mlflow.log_param("learning_rate", 0.01)
    mlflow.log_param("epochs", 100)
    
    # Training loop
    for epoch in range(100):
        loss = train_epoch()
        mlflow.log_metric("loss", loss, step=epoch)
    
    mlflow.sklearn.log_model(model, "model")
```

### Model Serving

#### REST API with FastAPI
```python
from fastapi import FastAPI
from pydantic import BaseModel
import joblib

app = FastAPI()
model = joblib.load("model.joblib")

class PredictionInput(BaseModel):
    features: List[float]

@app.post("/predict")
def predict(input_data: PredictionInput):
    prediction = model.predict([input_data.features])
    return {"prediction": prediction.tolist()}
```

#### Model Serving Patterns
* **Online Serving**
    - Real-time predictions
    - Low latency requirements
    - API endpoints

* **Batch Serving**
    - Large-scale predictions
    - Scheduled jobs
    - Data pipeline integration

## Containerization and Orchestration

### Docker for ML Applications
* **Dockerfile Example**
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY model/ model/
COPY src/ src/

EXPOSE 8000
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Kubernetes for ML Workloads
* **Key Concepts**
    - Pods vs Deployments
    - Services
    - ConfigMaps and Secrets
    - Resource management

* **Example Deployment**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: model-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: model-service
  template:
    metadata:
      labels:
        app: model-service
    spec:
      containers:
      - name: model-service
        image: model-service:v1
        resources:
          limits:
            memory: "1Gi"
            cpu: "500m"
        ports:
        - containerPort: 8000
```

## Monitoring and Observability

### Model Monitoring
* **Metrics to Track**
    - Model performance (accuracy, F1, etc.)
    - Prediction latency
    - Feature drift
    - Data quality

* **Monitoring Setup Example**
```python
from prometheus_client import Counter, Histogram
import time

PREDICTION_LATENCY = Histogram(
    'model_prediction_latency_seconds',
    'Time spent processing prediction'
)
PREDICTION_COUNTER = Counter(
    'model_predictions_total',
    'Total number of predictions'
)

@PREDICTION_LATENCY.time()
def predict(features):
    PREDICTION_COUNTER.inc()
    return model.predict(features)
```

### Data Quality Monitoring
* Schema validation
* Feature statistics
* Data drift detection
* Example with Great Expectations:
```python
import great_expectations as ge

def validate_dataset(df):
    expectation_suite = ge.core.ExpectationSuite(
        expectation_suite_name="my_suite"
    )
    df.expect_column_values_to_not_be_null("important_feature")
    df.expect_column_values_to_be_between(
        "numeric_feature", min_value=0, max_value=100
    )
```

## CI/CD for Machine Learning

### Continuous Integration
* Unit tests for ML code
* Integration tests
* Model validation tests
* Example test:
```python
def test_model_prediction_shape():
    model = load_model()
    X_test = load_test_data()
    predictions = model.predict(X_test)
    assert predictions.shape[0] == X_test.shape[0]
    assert predictions.shape[1] == n_classes
```

### Continuous Deployment
* **Deployment Strategies**
    - Blue-Green deployment
    - Canary releases
    - A/B testing

* **Example Feature Flag Configuration**
```python
def get_model_version(user_id):
    if feature_flag.is_enabled('new_model', user_id):
        return load_model('v2')
    return load_model('v1')
```

## Infrastructure as Code (IaC)

### Terraform Example
```hcl
resource "aws_sagemaker_model" "example" {
  name               = "my-model"
  execution_role_arn = aws_iam_role.example.arn

  primary_container {
    image          = "${aws_ecr_repository.example.repository_url}:latest"
    model_data_url = "s3://${aws_s3_bucket.example.bucket}/model.tar.gz"
  }
}
```

### Cloud-Specific Services

#### AWS ML Stack
* SageMaker
* Lambda
* ECS/EKS
* S3
* CloudWatch

#### GCP ML Stack
* Vertex AI
* Cloud Run
* GKE
* Cloud Storage
* Cloud Monitoring

## Best Practices

### Production ML Checklist
1. Model versioning and reproducibility
2. Automated testing pipeline
3. Monitoring and alerting
4. Documentation
5. Resource optimization
6. Security considerations
7. Compliance requirements

### Security Considerations
* Model access control
* Data encryption
* API authentication
* Example secure endpoint:
```python
from fastapi import FastAPI, Security
from fastapi.security import APIKeyHeader

app = FastAPI()
api_key_header = APIKeyHeader(name="X-API-Key")

@app.post("/predict")
async def predict(
    input_data: PredictionInput,
    api_key: str = Security(api_key_header)
):
    if not is_valid_api_key(api_key):
        raise HTTPException(status_code=403)
    return {"prediction": model.predict([input_data.features])}
```

### Performance Optimization
* Model quantization
* Batch prediction optimization
* Caching strategies
* Example caching:
```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def get_prediction(feature_tuple):
    return model.predict([feature_tuple])
```

## Interview Tips for MLOps
1. Be prepared to discuss end-to-end ML pipelines
2. Understand scalability challenges
3. Know common failure points and solutions
4. Be familiar with cloud services
5. Understanding of monitoring and observability
6. Knowledge of deployment strategies
7. Security best practices

Remember to focus on:
- Real-world problem-solving
- Trade-offs between different approaches
- Scalability considerations
- Cost optimization
- Team collaboration aspects