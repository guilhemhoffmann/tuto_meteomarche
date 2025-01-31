---
title: "Docker: First Python Script Run"
status: draft
planned_date: 2024-12-29
prerequisites: 
  - Basic Python knowledge
  - Docker installed on your system
  - Basic command line familiarity
follows_tutorial: "Docker Installation Basics"
learning_paths: 
  - "beginner"
  - "data_engineer"
difficulty: 🔧 Basic
estimated_time: 45
last_updated: 2024-12-28
---

# Docker: First Python Script Run

## Why This Matters 🎯

When developing Python applications, ensuring consistent environments across different machines is crucial. Docker solves this by containerizing your application. This tutorial shows you how to set up a development environment that allows for real-time code editing while running your Python scripts in a container.

## Prerequisites 📝

Before starting, ensure you have:
- Docker installed and running on your system
- A text editor of your choice
- Basic familiarity with Python
- Basic understanding of command line operations

## The Journey Begins 🚀

### Part 1: Setting Up Your Project

1. Create your project structure:
```bash
mkdir python-docker-dev
cd python-docker-dev
mkdir code
```

2. Create a simple Python script (`code/app.py`):
```python
def greet(name):
    return f"Hello, {name}! Welcome to Docker-powered Python development!"

if __name__ == "__main__":
    print(greet("Developer"))
```

3. Create a `requirements.txt` file:
```text
pytest==7.4.3
pandas==2.1.1
```

4. Create your Dockerfile:
```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install basic development tools
RUN apt-get update && \
    apt-get install -y git vim && \
    rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Don't copy the code - we'll use volumes
CMD ["python"]
```

### Part 2: Building Your Development Container

1. Build the Docker image:
```bash
docker build -t python-dev .
```

2. Run the container with volume mounting:
```bash
docker run -it --name python-dev-container \
    -v "$(pwd)/code:/app" \
    python-dev
```

> 💡 Pro Tip: The `-v` flag maps your local `code` directory to `/app` in the container. Any changes you make locally will instantly appear in the container!

### Part 3: Interactive Development

1. From your container's Python shell, you can import and run your code:
```python
>>> import app
>>> app.greet("Docker Learner")
'Hello, Docker Learner! Welcome to Docker-powered Python development!'
```

2. To run the script directly:
```bash
# In a new terminal, with the container running:
docker exec python-dev-container python app.py
```

3. For an interactive bash session:
```bash
docker exec -it python-dev-container bash
```

## Common Pitfalls and Solutions 🚧

1. "My changes aren't showing up in the container"
   - Ensure you're editing files in the mounted `code` directory
   - Check your volume mount path in the docker run command
   - Verify file permissions

2. "Container is already in use"
   ```bash
   # Remove the existing container
   docker rm python-dev-container
   # Then run the new one
   docker run -it --name python-dev-container -v "$(pwd)/code:/app" python-dev
   ```

## Hands-On Exercise 🏋️‍♂️

1. Modify the greeting function to include the current time:
```python
from datetime import datetime

def greet(name):
    current_time = datetime.now().strftime("%H:%M:%S")
    return f"Hello, {name}! It's {current_time}"
```

2. Save the file and run it in the container to see the changes take effect immediately!

## Checkpoint ✅

You should now be able to:
- [ ] Create a Dockerfile for Python development
- [ ] Build and run a Docker container with mounted volumes
- [ ] Execute Python code interactively in the container
- [ ] Make real-time changes to your code and see them reflected
- [ ] Use different methods to interact with your containerized application

## Next Steps 🎯

- Learn about Docker Compose for multi-container applications
- Explore debugging Python applications in Docker
- Discover best practices for production Docker images

## Key Takeaways 🎓

- Volume mounting (`-v`) enables real-time development in containers
- Interactive container sessions help with development and debugging
- Docker provides isolation while maintaining development flexibility
- Different ways to run Python code in containers (interactive shell, scripts, etc.)

## Reality Check 🎯

While this setup is perfect for development, remember that production containers should be built differently:
- Include the code in the image
- Minimize installed development tools
- Use specific entry points
- Consider security implications

---

Created: 2024-12-28
