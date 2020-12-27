---
layout: post
title: 'My Cloud Resume Challenge Experience'
date: 2020-11-15 16:39:36 +0000
categories: post
---

# My Cloud Resume Challenge Experience

...and what an experience it has been! If you want to find more detailed documentation of each stage of the process, I worked on this project on weekends and holidays as part of the [#100DaysOfCloud](https://github.com/notwaving/100DaysOfCloud) challenge. I'm a real learn-by-doing sort of person, and having a relatively loose brief and no deadline made it a joyful experience. The first requirement of the Cloud Resume Challenge was to be AWS Certified. My learning for this was largely theoretical (and the exam was _entirely_ theoretical), and I wanted to get some real-world practice that wouldn't cost a fortune. This Challenge is highly recommended for anyone who wants to put their new AWS certification to the test!

First, I looked at the Challenge as a whole and broke it down into front end, back end, and CI/CD tasks, then made cards on a Trello board. Seems a little OTT, but it really helped make things less overwhelming. The layout suggested a linear order to do things in, but I allowed myself the option to jump around, which was welcome when I got stuck in a particular area. Generally, I'd say approach it front end first, but there's a lot to be said for getting those GitHub actions in early, making updates to the live site instant (which was kind of the point of making a pipeline in the first place)...

#### On Documentation

Document everything - your future self will thank you for it. I was lucky in that I was already writing a daily log for the #100DaysOfCloud Challenge, because if I hadn't, I don't think I would've written any documentation at all. It came in particularly useful for me when deploying my second GitHub action, but with any luck you'll also be using what you've learned in future projects that use AWS and will have a reference for it all. Personally, I found very little out there in terms of official, user-friendly documentation; a significant chunk of the project was spent on Google, and other people's blogs and repos. I know this is kind of obvious and not an unusual experience but it's worth noting, as I'm pretty good at taking notes when studying, but for some reason it didn't immediately occur to me to do so for a _personal_ project, as if I didn't think I would apply what I'd learned to future _work_ projects. Document everything.

## Front End

#### The CV Itself

Others have deployed an extremely simple design of their CVs to their static site. Mine was a little more complex, but I have experience as a full-stack developer and I wanted to showcase those skills too. As well as the required HTML and CSS it uses Bootstrap for responsiveness (and some nice design features), and `smooth-scroll`, a JavaScript script to animate scrolling to anchor links.

#### Hosting as Amazon S3 static website

Pushing to an AWS S3 bucket was simple, but naming it got tricky once the domain name was bought and I was connecting it to CloudFront. I'd recommend you buy your domain name _before_ deploying to S3, as you can't change the bucket name once it's been created. However, it's easy enough to upload your site to a new S3 bucket with the intended name...

Setting up HTTPS with CloudFront was TOUGH - I think I identified three major gotchas. Eventually, I found a really decent tutorial on YouTube to help with this.

At this point I deployed the GitHub Action for the front end. Until I'd built the backend and had a dedicated API, I couldn't be certain that the JavaScript counter code would work. Having a pipeline in place also saved me having to manually update my S3 bucket every time I made a change to the this JavaScript file. I wrote a function with a placeholder for the API to the database, and hard-coded a visitor counter into the HTML.

## Back End

The biggest learning curve in the Challenge was in using a SAM template to deploy the DynamoDB database, the API Gateway, Lambda (and the IAM role that allows the lambda function to make updates to the database). I see why it's more expedient in the long run, but I found the current SAM CLI documentation hard to follow, and spent a lot of time cobbling things together in order to get it to work (like, syntax??!!). A CORS error took me forever to fix because the documentation only provided snippets of the template YAML, and didn't show how components work together as a whole for context.

Testing with boto3 was rudimentary; aware that people have whole careers in this area, I got it working and moved on quickly.

Linking up the front and back ends via the JavaScript counter was frustrating, particularly as clicking on the raw API Gateway link always returned an updated counter database!

## CI/CD

This was much more straightforward than expected - the challenge with GitHub Actions was idenfifying that an Action was safe, actively maintined, and did what it said it would do. It took a couple of attempts to find one that actually included CloudFront cache invalidation... I alse took the note _not_ to commit AWS credentials to source control, and put them straight into GitHub Secrets. As I said at the top of this post, documenting what I did in the front end helped _immensely_ with the back end.

## Next

- Optimise the front end. [Lighthouse](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk?hl=en) has suggested I minify all JavaScript for a start.

- Learn the options for the backend services in the AWS Console. This would be a more visual way to learn features of Lambda, API Gateway and DynamoDB, as the current SAM CLI and template docs weren't easy to navigate and implement.

- Implement more thorough testing. As I said, I avoided jumping down a deep rabbit hole in this area. I'm currently on a course learning Java and deepening my JavaScript skills - adding Python right now would be the straw that broke the camel's back. However, in the future I'll be using Python as my main scripting language so it'll all come around.
