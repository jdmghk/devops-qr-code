# Base stage: Sets up the foundation for other stages
FROM node:18-alpine as base
# Install necessary build tools and dependencies
RUN apk add --no-cache g++ make py3-pip libc6-compat
# Set the working directory
WORKDIR /app
# Copy package.json and package-lock.json (if exists)
COPY package*.json ./
# Expose port 3000 for the application
EXPOSE 3000

# Builder stage: Used to build the Next.js application
FROM base as builder
WORKDIR /app
# Copy all source files
COPY . .
# Build the Next.js application
RUN npm run build


# Production stage: Sets up the production environment
FROM base as production
WORKDIR /app

# Set NODE_ENV to production for optimized performance
ENV NODE_ENV=production
# Install only production dependencies
RUN npm ci

# Create a non-root user for better security
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001
USER nextjs

# Copy built files and dependencies from builder stage
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/public ./public

# Command to start the production server
CMD npm start

# Development stage: Sets up the development environment
FROM base as dev
# Set NODE_ENV to development
ENV NODE_ENV=development
# Install all dependencies, including devDependencies
RUN npm install 
# Copy all source files
COPY . .
# Command to start the development server
CMD npm run dev




