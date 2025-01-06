# Integration

Integration tests, e2e tests, tend to interact with the entry points.

In Ruby, this can mean using `Rack::Test`.

For setup, the spec/spec_helper.rb:

```ruby
require 'rack/test'
require 'rspec'
require './app.rb'

RSpec.configure do |config|
  config.include Rack::Test::Methods

  def app
    Sinatra::Application
  end
end
```

And a test, spec/healthcheck_controller_spec.rb:

```ruby
require 'spec_helper'

describe 'Healthcheck Endpoint' do
  it 'returns a 200 status' do
    get '/healthcheck'
    expect(last_response.status).to eq(200)
    expect(JSON.parse(last_response.body)).to eq({ 'status' => 'ok', 'message' => 'Service is running' })
  end
end
```
