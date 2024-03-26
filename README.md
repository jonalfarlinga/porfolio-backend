# My Portfolio

1. [The Cloud Resume Challenge](#the-cloud-resume-challenge)
2. [Tech Stack](#tech-stack)
3. [The Work](#the-work)
4. [The Experience](#the-experience)
5. [Author and Acknowledgment](#author-and-acknowledgment)
6. [Dev Roadmap](#dev-roadmap)

## The Cloud Resume Challenge
This Cloud Resume Challenge requires the developer to deploy a frontend and backend using Github Actions and the Azure Cloud. When complete, the full pipeline is automated and keeps the page updated when new code is pushed to the repository.

The root of this repository contains the `function_app` for my Azure Functions API and the rest of the module files are in `/api`. The `/public` directory contains the index and `/dev` contains the backend dev environment for my this project. The live portfolio/resume can be found [here](portolio.denny-bucklin.net).


## Tech Stack
#### HTMX, CSS(Bootstrap), Python(Azure Functions), Azure Tables
#### Frontend
The main page is built with **HTML** and **CSS**, utilizing Bootstrap for styling.
#### Backend
I use **HTMX** and a **Python** backend to serve the separate page views.
#### Hosting Service
The backend API is served from an **Azure Functions App**
#### Database
The backend interacts with an **Azure Table** in blob storage to get and update a continuous count of page loads.

## The Work
To build my cloud-based online resume, I first wrote the site in HTMX and and CSS. This took longer than expected, as I spent a long time styling the page manually.

Next I created a Static Web App in my Azure Storage Account, and uploaded the `index.html`, `index.css`, and `/img` assets to the `$web` container. By navigating to the storage account url, I could see the live webpage, although it had now content without a backend.

For a public endpoint, I would want a custom url, so I purchased a domain on Cloudflare. Then I created an Azure CDN account and endpoint and created a CNAME on my DNS to associate the Azure endpoint to the domain name. Finally, I could create a custom domain on my CDN account, completing the bridge from my Azure Storage Static Page to my custom domain.

Next, I needed to upload a set of functions to an Azure Function App. I found the best way to do this was to use the Azure plugin for VSCode to make a test environment. Once I was sure that the functions worked, I uploaded them via Github Actions to the Azure Functions App.

I added a unit test for the Azure Tables `increment` function and I wrote a Github Workflows to test and push project changes to the azure storage account's static web page.

![App diagram](./public/img/App_diagram.png)
## The Experience
- I decided to learn a new technology with this project: **HTMX**. It was a lot of fun, even though I've only scratched the surface. My first real hurdle was was trying to change the `#content` div and the `nav` items with one request, but once I discovered the OOB swap, it was a breeze.

- Using a backend to serve the **HTMX** fetches means that I couldn't follow the challenge exactly. I had to have the API up and runnning in order to have the website running, even without a site-visit counter or database to contact.

- I struggled to set up a `CNAME` for my custom domain. I was able to add the record on Cloudflare all right, but it would never show up on nslookup.io. Finally, after much fiddling, I found out that the Cloudflare proxy was preventing Azure from reaching the `CNAME` record. After correcting this, I rapidly fixed the issues and brought the site up live through my custom server.

- There are a number of environment variables in the Github Actions pipeline. I tried several ways to add a necessary key to the pipeline, and kept having issues, until I learned that Github Actions variables are not accessible by my functions. _I looked up where to add variables in Azure Functions, and found out that the variable I was triyng to add already exists in the Functions environment._ **RTFM**

- Azure Functions Python documentation leaves a lot to be desired. I couldn't get the CORS properties to work in the portal, which was a common complaint online. Instead I had to make my function return its CORS header explicitly. It currently returns an allowance for all CORS hosts because neither the custom domain nor the storage endpoint were allowing the POST request to complete. I also haven't been able to discover how to restrict access methods on functions, although I believe I can cause a function to respond differently to different request methods.

- I have to be careful about the products I use from Azure. The wrong subscription can add up quickly.

- I struggled to understand ARM templates, but I was able to CI the frontend using Github actions. I don't even have to create a separate repository. When I push to main, one workflow pushes to Azure Functions, and another uploads the `public/` folder to Azure Storage `$web`.

- I successfully deployed a storage account and endpoint using ARM templates, but the Function app gives me trouble. I combined the storage account and endpoint into one template since the function app is dependent on a storage account.

## Author and acknowledgment
This project is written by and for Denny Bucklin.

With thanks to my wife and the Base 12 team for criticizing my color choices.

The project follows the path laid out in **The Cloud Resume Challenge Guidebook** by [Forrest Brazeal](@forrestbrazeal).

## Dev Roadmap
- <strike>I would like to add Github Actions to keep `index.html` and `index.css` automatically updated on Azure.</strike>

- I want to add a testimonials page

- I plan to add a tab with my experiences building this project

- Updates for project tags
  - [X] Technical Link
  - [X] Website Link
  - [X] Description of Project
  - [X] Tech Stack
  - [X] Image File Name
  - [ ] Project Demos
