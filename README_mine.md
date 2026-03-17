# letta-terminalbench
Official code for Letta on [Terminal-Bench](https://www.tbench.ai/news/announcement). 

Check out our technical blog for more information: [Blog](https://www.letta.com/blog/terminal-bench)

## Quick Start
First, install the dependencies:
```bash
pip install -r requirements.txt
```

Second, create the `.env` file and add your anthropic api key in the file.
```bash
ANTHROPIC_API_KEY=your_anthropic_api_key
```

Then, run the Letta server with Docker:
```bash
docker run \
  -v ~/.letta/.persist/pgdata:/var/lib/postgresql/data \
  -p 8283:8283 \
  --env-file .env \
  letta/letta:latest
```

If you have the proxy, please run this command instead:
```bash
docker run \
  -v ~/.letta/.persist/pgdata:/var/lib/postgresql/data \
  -p 8283:8283 \
  --env-file .env \
  -e HTTP_PROXY=http://host.docker.internal:7897 \
  -e HTTPS_PROXY=http://host.docker.internal:7897 \
  -e NO_PROXY=localhost,127.0.0.1,::1 \
  letta/letta:latest
```

The detailed document is [here](https://docs.letta.com/guides/docker/).


Finally, tun the following command to run the evaluation.

```bash
tb run \
    --dataset-name terminal-bench-core \
    --dataset-version 0.1.1 \
    --agent-import-path letta-agent.letta_agent_v1:LettaAgent \
    --n-concurrent 4 --model anthropic/claude-sonnet-4-20250514 

# note this script supports --model anthropic/*. However, one can modify LlmConfig to use other models.
```

## Others
To enter the docker shell, you can run the following command.
First, find the docker_id by running:
```bash
docker ps
```
Then, run the following command to enter the docker shell:
```bash
docker exec -it <docker_id> bash
```
