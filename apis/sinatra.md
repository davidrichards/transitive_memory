# Sinatra

## Gemfile

```ruby
gem 'sinatra'
gem 'sinatra-contrib' # For JSON handling and modular structure
```

## App

```ruby
require 'sinatra'
require 'sinatra/json'
require 'json'

# Health check endpoint
get '/healthcheck' do
  status 200
  json status: 'ok', message: 'Service is running'
end

# GET /hello endpoint

get '/hello' do
  name = params[:name] || 'World'
  json message: "Hello, #{name}!"
end

# POST /do_work endpoint

post '/do_work' do
  begin
    payload = JSON.parse(request.body.read)
    work_description = payload['work_description'] || 'No work described'
    # Simulate doing work
    result = "Processed work: #{work_description}"

    status 200
    json success: true, result: result
  rescue JSON::ParserError
    status 400
    json success: false, error: 'Invalid JSON payload'
  end
end

# API Documentation
get '/' do
  api_docs = {
    endpoints: [
      {
        path: '/healthcheck',
        method: 'GET',
        description: 'Check service health',
        response: { status: 'ok', message: 'Service is running' }
      },
      {
        path: '/hello',
        method: 'GET',
        params: { name: 'Optional name parameter (string)' },
        description: 'Returns a greeting message',
        response: { message: 'Hello, <name>!' }
      },
      {
        path: '/do_work',
        method: 'POST',
        body: { work_description: 'Description of the work to process (string)' },
        description: 'Processes a given work task and returns the result',
        response: { success: true, result: 'Processed work: <description>' }
      }
    ]
  }

  content_type :json
  JSON.pretty_generate(api_docs)
end

# Start the application
# To run: ruby app.rb
if __FILE__ == $0
  set :port, 4567
  run!
end
```

To run:

```bash
ruby app.rb
```

To GET:

```bash
curl -X GET http://localhost:4567/hello
```

Or:

```bash
curl -X GET "http://localhost:4567/hello?name=David"
```

To POST:

```bash
curl -X POST http://localhost:4567/do_work -H "Content-Type: application/json" -d '{"work_description": "Example Task"}'
```

## Containerize

Dockerfile:

```dockerfile
# Use the official Ruby image
FROM ruby:3.2

# Set working directory
WORKDIR /usr/src/app

# Copy the application files
COPY . .

# Install dependencies
RUN gem install bundler && bundle install

# Expose the application port
EXPOSE 4567

# Start the Sinatra application
CMD ["ruby", "app.rb"]
```

docker-compose.yml:

```dockerfile
version: '3.8'

services:
  sinatra-app:
    build: .
    ports:
      - "4567:4567"
    volumes:
      - .:/usr/src/app
    environment:
      - PORT=4567
```

build and run:

```bash
docker build -t sinatra-app .
docker run -p 4567:4567 sinatra-app
```

with docker-compose:

```bash
docker-compose up --build
```

## Interface Tests

```ruby
RSpec.describe 'Sinatra App' do
  include Rack::Test::Methods

  def app
    Sinatra::Application
  end

  it 'returns 200 for healthcheck' do
    get '/healthcheck'
    expect(last_response.status).to eq(200)
    expect(JSON.parse(last_response.body)).to eq({ 'status' => 'ok', 'message' => 'Service is running' })
  end

  it 'returns a greeting from /hello' do
    get '/hello', name: 'David'
    expect(last_response.status).to eq(200)
    expect(JSON.parse(last_response.body)).to eq({ 'message' => 'Hello, David!' })
  end

  it 'returns a default greeting from /hello when no name is provided' do
    get '/hello'
    expect(last_response.status).to eq(200)
    expect(JSON.parse(last_response.body)).to eq({ 'message' => 'Hello, World!' })
  end

  it 'processes work in /do_work' do
    post '/do_work', { work_description: 'Test Task' }.to_json, { 'CONTENT_TYPE' => 'application/json' }
    expect(last_response.status).to eq(200)
    expect(JSON.parse(last_response.body)).to eq({ 'success' => true, 'result' => 'Processed work: Test Task' })
  end

  it 'returns an error for invalid JSON in /do_work' do
    post '/do_work', 'invalid_json', { 'CONTENT_TYPE' => 'application/json' }
    expect(last_response.status).to eq(400)
    expect(JSON.parse(last_response.body)).to eq({ 'success' => false, 'error' => 'Invalid JSON payload' })
  end
end
```
