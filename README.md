# Cleric Query Agent Assignment

## My Approach

My approach to building this Kubernetes (K8s) Querying Agent leveraging an LLM started with investigating how we can leverage LLM models like OpenAI GPT-4o to query about an application deployed on a K8s cluster. Since natural language can be weird even for an LLM to interpret, it can cause unwanted side-effects when going from our query to LLM inference to performing the actual K8s query.

After playing around and prompting OpenAI's models to best understand its strengths and limitations, I found that giving the model a role and then prompting it infer intent was the best way to normalize a specific request. This would mean that the LLM would infer that the queries "Get num pods in default namespace" and "Get the count of pods for the default namespace" have an intent along the lines "The user wants to get the number of pods in the default namespace" including the relevant `kubectl` command.

To further make the inference more robust, I added an instruction to possibly categorize the query as vague and tell the user that the agent could not understand its intent. This is good to eliminate outliers and ensures the user is querying relevant requests to the agent.

## Results

An AI agent tailored to knowledge about K8s and can process queries and answer questions about applications deployed on a K8s cluster, including:
- Pod count and status
- Service count
- Deployment count and details
- Node count
- Intent recognition regarding K8s
- Query validator

## What I learned

- Better understanding the strengths and limitations of LLM models
- How to integrate LLM models to query about an application deployed on a K8s cluster
- LLM prompting techniques including role-based prompting

## What to improve

- The scalability of the agent capabilities (could maybe define an interface of commands that the user could query, but in a slightly more LLM-interpretable way)
- The agent itself (it's not perfect, and probably doesn't cover all the K8s querying cases)

## Introduction
This document outlines the requirements and guidelines for the Cleric Query Agent Assignment. Your task is to develop an AI agent capable of accurately answering queries about applications deployed on a Kubernetes cluster.

## Objective
Create an AI agent that interacts with a Kubernetes cluster to answer queries about its deployed applications.

## Assignment Details

### Technical Requirements
- Use Python 3.10
- The kubeconfig file will be located at `~/.kube/config`
- Utilize GPT-4 or a model with comparable performance for natural language processing

### API Specifications
Your agent should provide a POST endpoint for query submission:
- URL: `http://localhost:8000/query`
- Port: 8000
- Payload format:
  ```json
  {
      "query": "How many pods are in the default namespace?"
  }
  ```
- Response format (using Pydantic):
  ```python
  from pydantic import BaseModel

  class QueryResponse(BaseModel):
      query: str
      answer: str
  ```

### Scope of Queries
- Queries will require only read actions from your agent
- Topics may include status, information, or logs of resources deployed on Minikube
- Answers will not change dynamically
- Approximately 10 queries will be asked
- Queries are independent of each other
- Return only the answer, without identifiers (e.g., "mongodb" instead of "mongodb-56c598c8fc")

## Submission Guidelines
Submit your repository to [submission link](https://query-agent-assignment-validator-347704744679.us-central1.run.app/)
 - The validator will return your score within a few minutes
 - Use logging if you want to check your outputs, make sure write logs to `agent.log`
 - If you encounter errors, wait a few minutes before retrying
 - Do not refresh the browser to avoid losing your session
 - Make sure to note your `Submission ID` for the Google form for the final submission.

### Submission Requirements
1. GitHub Repository
   - Include a `README.md` file describing your approach
   - Ensure your main script is named `main.py`
2. Loom Video
   - Keep it informal and personal
   - Focus on your motivation and background
3. Submit the `Loom video` and `submission ID` for the final submission on this [Google Form Link](https://docs.google.com/forms/d/e/1FAIpQLScUpEklWG-hYCIsBFo9pD-SAtyaCsevhQSz6XRLKkLV_K3KuQ/viewform?usp=sf_link)

## Submission Deadline:
There is no specific deadline for submitting this assignment;  however, we expect it to be completed within a **reasonable amount of time**. 
- We understand that personal and professional responsibilities can take priority, 
and we encourage you to balance this assignment with your other commitments. 
- Please aim to submit your work once you feel confident in your solution and it aligns with the objectives.

## Testing Your Agent
We recommend testing your agent locally before submission:
1. Install [Minikube](https://minikube.sigs.k8s.io/docs/start/)
2. Set up a local Kubernetes cluster
3. Deploy sample applications
4. Run your agent and test with sample queries

## Evaluation Criteria
- Accuracy of answers
- Code quality and organization
- Clarity of explanation in README and video

## Example Queries and Responses
1. Q: "Which pod is spawned by my-deployment?"
   A: "my-pod"
2. Q: "What is the status of the pod named 'example-pod'?"
   A: "Running"
3. Q: "How many nodes are there in the cluster?"
   A: "2"
