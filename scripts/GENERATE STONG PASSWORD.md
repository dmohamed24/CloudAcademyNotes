
**Hey everyone! Welcome to my Architecture Evolution Project.**

The goal of this project is to bring together everything I’ve learned across cloud and DevOps — and actually put it into practice.  
From Node.js and Docker, to AWS, Infrastructure as Code, and CI/CD — I wanted to build an end-to-end app that ties all of it together.

As a Software Engineer, I usually focus on building applications — not so much the deployment side. So this was a great opportunity to step out of my usual lane and go deep into the infrastructure and automation side of things.

---

i'll be using this simple app: a **Strong Password Generator** built with Node.js and Express as my strating point in builing this end-end project

📱 **[Insert Demo Here]**  
The app lets you:

- Enter an account or website name
    
- Click “Generate” to create a strong password
    
- View or hide saved passwords
    
- Delete entries  
    All data is saved in the browser’s local storage — no backend or database required for now.
    

---

From this basic starting point, I’ll be evolving the architecture step-by-step into a fully automated, highly available, and monitored cloud deployment.

In this video series, we’ll cover:

✅ Containerizing the app with Docker  
✅ Setting up a CI/CD pipeline using GitHub Actions  
✅ Deploying to AWS EC2 using Terraform  
✅ Adding a Load Balancer and Auto Scaling Group for High Availability  
✅ Monitoring and alerting with CloudWatch

---

My goal with this project is to **deepen my understanding of cloud-native architecture** and **see how all the pieces come together in a real-world environment**.

Let’s get started!




## DOCKERFILE

This project uses a multi-stage Docker build, which means we define separate build environments in a single file — one for development and one for production

1. The base stage sets up the Node environment and installs dependencies.
   - Uses a lightweight Alpine-based Node.js 20 image as your base. The alias `base` lets you reuse this setup in later stages.
   -  Alpine is a minimal version of Linux — it’s super lightweight and optimized for security and performance. Using Alpine results in samller Docker images being created ehich results in quicker build time which is importatn when pushing and pulling to docerk hub
   - Sets the working directory inside the container.
   - `COPY package*.json ./` Copies both `package.json` and `package-lock.json` (if present) into the container — used for installing dependencies.

2. The development stage adds dev tools and runs the app with npm run dev.
   - Starts from the shared `base` stage. 
   - `ENV NODE_ENV=development` Sets an environment variable to enable dev-only behavior (e.g., more logging, error messages).
   - Here we install all dependencies (npm install), including dev dependencies needed for local dev tools like `nodemon`
   - Copies your entire source code into the container.
   - Starts the dev server — likely with something like `nodemon` for auto-reloading.

3. The production stage strips out dev dependencies, copies only the necessary code, and runs the app with node index.js.

   - `FROM base AS production` Starts fresh from the `base`, keeping the image lean
   - Ensures production mode is enabled (e.g., disables dev-only features).
   - Instead of npm install, we use npm ci — : This is a clean install that uses the exact versions from package-lock.json. It’s faster, more ideal for production because it avoids modifying lockfiles or resolving dependencies again. 
   - We copy only the required source code (not the full dev environment). We also change the ownership of these files to the non-root node user, improving security — we don’t want to run production containers as root
   - USER node ensures that the app is executed as the less-privileged non-root user — best practice for container security.
   - We expose port 3000 and run the production server with node index.js.

  

This helps keep the final production image lean and secure.”
## DOCKER -COMPOSE FILE

```
version: '3.8'
```

✅ **Specifies the version of the Compose file format you're using.**  
Version `3.8` is compatible with modern Docker and Docker Compose, and it ensures features like volume mounting and service configs behave consistently.

```
services:
  app:
```
✅ **Defines the services (i.e., containers) to run.**  
Here you have only one service, called `app`, which will be the container that runs your Node.js application.


```
    build:
      context: .
      target: development
```
🔨 **Builds the Docker image using your Dockerfile.**

- `context: .` — This means Docker will use the current directory (`.`) as the build context, i.e., what files it has access to when building the image.
    
- `target: development` — You're telling Docker Compose to use the `development` **build stage** from your Dockerfile 


```
    ports:
      - '3000:3000'
```

🌐 **Maps port 3000 on your machine to port 3000 in the container.**  
This allows you to access the app in your browser at `http://localhost:3000`.

```
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
```

- `.:/usr/src/app` — Mounts your current project directory (on your host machine) into the container’s working directory. This enables **live code changes** without rebuilding the container.
    
- `/usr/src/app/node_modules` — An _anonymous_ volume that ensures your container's `node_modules` (which are OS-specific) don’t get overridden by the host’s empty `node_modules` directory. Prevents dependency conflicts between your host and container.

```
    command: npm run dev
```
This runs `npm run dev` (usually `nodemon` or similar) so your app reloads on file changes — ideal for local development.


DEMO

docker build . --target=development -t myapp-dev
docker run -d -p 3000:3000 myapp-dev

DOCKER-COMPOSE BUILD

DCOKER COMPOSE UP


## 🎤 Transcript: "Lessons Learned & Fixes in My CI/CD Pipeline"

> So during the process of setting up and testing this CI/CD pipeline using GitHub Actions and Docker on an EC2 instance, I ran into a couple of tricky issues that really highlight how important it is to understand how your app is built, structured, and run inside containers.

---

### 🐳 1. Dockerfile — Fixing the Copy Instructions and Entry Point

> Initially, in my Dockerfile, I had this line:
> 
> `COPY --chown=node:node src public ./`
> 
> At first glance, it looks fine — I’m just copying the `src` and `public` folders into the container. But the issue was **where** those folders were being copied to. Without explicitly stating the destination directory, they were being dumped into the root working directory — which in this case was `/usr/src` — but not always where the app expected them to be.
> 
> The fix was to split it out properly:
> 
> `COPY --chown=node:node public ./public COPY --chown=node:node src ./src`
> 
> This made sure both folders were copied into predictable paths (`/usr/src/public` and `/usr/src/src`) where the app was actually trying to serve static files from.

> The next Dockerfile issue was the **start command**. Originally, I had:
> 
> `CMD ["node", "index.js"]`
> 
> But the `index.js` file lives inside the `src/` directory, not the root. So naturally, the container would start and immediately crash with a “file not found” error.
> 
> The fix:
> 
> `CMD ["node", "src/index.js"]`
> 
> That ensured the container runs the actual app entry point correctly.


### 🧠 2. `index.js` — Fixing Static Path Resolution and Binding

> In the app itself, I ran into a common but frustrating issue around serving static files.
> 
> Initially, the route for `/` was doing this:
> 
> `res.sendFile(path.join(__dirname, '../../public/index.html'));`
> 
> But inside the container, this relative path didn’t resolve correctly — and I kept seeing repeated `ENOENT` errors saying the file didn’t exist.
> 
> The fix was to change it to:
> 
> `res.sendFile(path.join(__dirname, 'public/index.html'));`
> 
> Since the `public` folder is now being copied to `/usr/src/public`, and the working directory is `/usr/src`, this relative path works perfectly.
> 
> That change completely resolved the "not found" errors I was seeing in the browser.



### 🌐 Binding to All Interfaces

> The final subtle issue was with how the Express server was listening for connections.
> 
> I had:
> 
> `app.listen(PORT, () => { ... });`
> 
> But when running in a Docker container, **you must explicitly bind to `0.0.0.0`**, otherwise the server only listens on `localhost` **inside the container**, and it’s unreachable from the outside — including from the EC2 host itself. In other words “listens on all IP addresses available to the app”
> 
> So the fix was:
> 
> `app.listen(PORT, '0.0.0.0', () => { ... });`
> 
> That made the containerized app accessible over port 3000 on the EC2 instance.

es, a server binding to `0.0.0.0` means it will listen for and accept connections on all available network interfaces of the host machine. Effectively, it's binding to all IPv4 addresses assigned to the machine. This is a common practice for servers that need to be accessible from any network interface on the system

