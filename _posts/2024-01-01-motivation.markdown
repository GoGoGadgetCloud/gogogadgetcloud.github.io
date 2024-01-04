---
layout: post
title:  "Motivation for a new GO Framework"
date:   2024-01-01 13:44:47 +0100
categories: gadget cloud aws go
---

There is a [demo project](https://github.com/GoGoGadgetCloud/gadget-demo) showing off how we could mix and match deployment code with business code. 

# Motivation

Using AWS Lambdas requires the least amount of human power and expertise to operate - even at large productive scale deployments.

The are three main things prohibiting the use of Lambdas:

## The problem of complex set-up

Using cloud native setups you need to setup a lot of moving parts. The simplest example of a hello-world lambda connected to an API Gateway requires approximately 200 lines of Cloudformation YAML to get working.

Either you have an infrastructure team in your organisation hosting and maintaing CI/CD stacks, you use frameworks like Serverless or SST or you become a fullstack DevOps team, which works with Terraform/CDK/Cloudformation, keep it insync with your application code and wrap it all together with a CI/CD stack.

All of this limits the usage of Lambda to "larger" setups or forces you to use Python / JS (the tools the _cool_ frameworks support.)

## The problem of prohibitive resource pricing.

Let us face it: Serverless is the most expensive way to _buy_ CPU cycles and _rent_ RAM circuits. The paradox is, that hardware costs are in your face when using _serverless_. Python and Javascript are the most popular languages running on Lambda:

![popularity](/images/popularity.png "Language Popularity (by datadog)"){:class="img-responsive"}

Taken from [Datadog](https://www.datadoghq.com/state-of-serverless/) Updated August 2023
{.info}

Oddly enought these languages are not very resource efficient:

![performance](/images/all_languages_performance.png "Performance in Execution Time with Various Memory Profiles"){:class="img-responsive"}

Taken from [Scanner.dev](https://blog.scanner.dev/serverless-speed-rust-vs-go-java-python-in-aws-lambda-functions/) Updated March 2023
{.info}

We should not dive too deep into a single benchmark, but the results all point into the same direction: You will only require 50% of the memory and be 4 to 6 times faster using Go, than compared to Python or Java. 

Using Python or NodeJS (similar performance) therefore is only suited for small deployments, where the development costs outweigh the operation costs. If your application becomes popular, you need to either move out of Lambdas (which will offset your development cost benefit) or recode your appliction (which will also offset your development costs). Again this is limiting the use case for Lambda.

Go and Rust have quite comparative performance, but have a large difference towards the developer. Rust works with References and Borrowing to keep your memory clean - this is a very elegant and extremely efficient mechanism. It is to no surprise that the Linux Kernel is seeing more and more Rust modules. But for the _run of the mill_ developer, Rust hast an extremely steep learning curve.

Go on the other hand is built with a garbage collector and is extremely simple. It beging developed as "the go-to language for k8s" shows, as its great in _gluing together_ APIs to make new applications. For a Java or Python developer it is a simple migration. 

### Remote Execution

Your application does not run on your machine. This makes development awkard and debugging a pain. Interpreted languages are at a disadvantage here, as you do not "know" that the application will work. You need to rely on the IDE to point out issues, write siginicant unit test just to check your runtime types and depencies. 

Ideally you want to work with compiled langauges with a strong type checking to prevent as many _gotchas_ as possible and reduce your unit test requirements. 

SST has created the gold standard with its development mode and there is no need to limit this to SST and can be easily replicated to allow AWS triggers to find your machine via tha IOT-GW and allows you to run and debug the code from the comfort of your IDE. Only a small amount of effort is required to give you full breakpoint control.

A full remote deployment will always take minutes to work and there is no need to interrupt the developer flow dozens of times a day.

## Thesis

Go favours the small framework and the simple and concise. We want to leverage amazing resource efficiency of go to reduce operational costs. We want to create a framework which enhances the developer productiviy, even for small teams without infrastructure support or DevOps experts. We want to use the go compiler to the fullest extent to reduce the testing requirements.