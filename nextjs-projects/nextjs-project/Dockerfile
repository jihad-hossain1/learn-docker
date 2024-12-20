# Step 1: Use official Node.js image to build the Next.js app
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy the rest of the project files
COPY . .

# Build the Next.js app
RUN npm run build

# Step 2: Create production image
# FROM node:18-alpine

# # Set working directory
# WORKDIR /app

# # Install production dependencies
# COPY --from=builder /app/package.json /app/package-lock.json ./
# RUN npm install --production

# # Copy built Next.js app from builder stage
# COPY --from=builder /app/.next ./.next
# COPY --from=builder /app/public ./public

# Expose port for Next.js
EXPOSE 3000

# Start the Next.js app
CMD ["npm", "start"]
