# Containerization

## Python

A good starter container for Python development.

```python
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt ./
RUN apt-get update && apt-get install -y git curl vim tree

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install -r requirements.txt

COPY .bashrc /root/.bashrc

COPY src/ ./src/
CMD ["tail", "-f", "/dev/null"]
```

## Ruby

```ruby
FROM ruby:3.2-slim

# Set the working directory
WORKDIR /app

# Copy Gemfile and Gemfile.lock to the container
COPY Gemfile Gemfile.lock ./

# Install basic tools and dependencies
RUN apt-get update && apt-get install -y \
    git curl vim tree build-essential libsqlite3-dev

# Install the Ruby gems
RUN bundle install

# Optional: Copy custom shell configuration (if you have one)
COPY .bashrc /root/.bashrc

# Copy your application source code
COPY src/ ./src/

# Keep the container running for interactive work
CMD ["tail", "-f", "/dev/null"]
```

## Elixir

```elixir
# Use the official Elixir image with OTP (Erlang) installed
FROM elixir:1.15-slim

# Set the working directory
WORKDIR /app

# Install basic tools and dependencies
RUN apt-get update && apt-get install -y \
    git curl vim tree build-essential

# Install Hex and Rebar (Elixir build tools)
RUN mix local.hex --force && \
    mix local.rebar --force

# Copy your project dependencies
COPY mix.exs mix.lock ./

# Fetch and compile Elixir dependencies
RUN mix deps.get && mix deps.compile

# Optional: Copy custom shell configuration (if you have one)
COPY .bashrc /root/.bashrc

# Copy your application source code
COPY lib/ ./lib/

# Keep the container running for interactive work
CMD ["tail", "-f", "/dev/null"]
```
