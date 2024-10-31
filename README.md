# gen-al-demo
Gen AL Demo with Istio Ambient

## Prerequisites

- A Kubernetes cluster, for example a [kind](https://kind.sigs.k8s.io/) cluster.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

Pull the LLaVa model by running

With Ollama installed on your machine, you will need to pull the LLaVa model by running

```sh
ollama pull llava
```

You can verify that the model is installed by running

```sh
ollama list

NAME            ID           SIZE   MODIFIED
llava:latest    8dd30f6b0cb1 4.7 GB 17 seconds ago 
```

## Startup

We have crafted a few scripts to make this demo run as quickly as possible on your machine once you've installed the prerequisites.

This script will:

- Create a kind cluster
- Install a simple curl client, an [ollama](https://ollama.com/) service and the demo service.
  - Ollama is a Language Model as a Service (LMaaS) that provides a RESTful API for interacting with large language models. It's a great way to get started with LLMs without having to worry about the infrastructure.

```sh
./startup.sh
```

## Load the LLM models used in the demo

The following two LLM models are used in the demo:
- LLaVa (Large Language and Vision Assistant)
- Llama (Large Language Model Meta AI) 3.2

Load the two models:

```sh
kubectl exec -it deploy/client -- curl http://ollama.ollama:80/api/pull -d '{"name": "llama3.2"}'
kubectl exec -it deploy/client -- curl http://ollama.ollama:80/api/pull -d '{"name": "llava"}'
```

## Install Istio and Enroll your apps to Istio ambient

We use [Istio](https://istio.io) to secure, observe and control the traffic among the microservices in the cluster.

```sh
./install-istio.sh
```

## Access the demo app

Use port-forwarding to help us access the demo app:

```sh
kubectl port-forward svc/demo 8001:8001
```

To access the demo app, open your browser and navigate to [http://localhost:8001](http://localhost:8001)

## Shutdown

To shut down the demo, run the following command, which will:

- Remove the Kubernetes manifests
- Remove the port-forwarding
- Delete the kind cluster

```sh
./shutdown.sh
```

## Operating System Information

This demo has been tested on the following operating systems and will work if you have the prerequisites installed.

- macOS M2

## Credits
A portion of the demo in this repo was inspired by the [github.com/cncf/llm-in-action](github.com/cncf/llm-in-action) repo.

