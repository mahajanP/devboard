# Stage 1 - Build the application
FROM dhi.io/node:26-dev AS build

WORKDIR /app

# Copy dependency files first for better caching
COPY package*.json ./

# Install dependencies
RUN npm install --legacy-peer-deps --verbose

# Install Vite only if it's not already present
RUN npm list vite > /dev/null 2>&1 || npm install --no-save vite@5.4.11

# Copy application source
COPY . .

# Build the application
RUN npm run build

# Stage 2 - Runtime image
FROM dhi.io/node:26

WORKDIR /app

# Copy build artifacts and dependencies
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/package*.json ./

EXPOSE 4173

# Start Vite preview server
CMD ["./node_modules/.bin/vite", "preview", "--host", "0.0.0.0", "--port", "4173"]
