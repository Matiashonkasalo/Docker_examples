# Use Ruby 3.1.0 as the base image
FROM ruby:3.1.0

# Install dependencies (build tools, PostgreSQL libraries, Node.js for JS runtime)
RUN apt-get update -qq && \
    apt-get install -y build-essential libpq-dev nodejs

# Set working directory inside the container
WORKDIR /usr/src/app

# Install correct Bundler version
RUN gem install bundler:2.3.3

# Copy dependency files and install gems
COPY Gemfile* ./
RUN bundle install

# Copy the rest of the app
COPY . .

# Prepare for production: run migrations & precompile assets
RUN rails db:migrate RAILS_ENV=production
RUN rake assets:precompile

# Expose port 3000 (Rails default)
EXPOSE 3000

# Run the Rails app in production mode
CMD ["rails", "s", "-e", "production"]

