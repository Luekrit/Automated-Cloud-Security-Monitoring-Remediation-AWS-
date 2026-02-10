<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build an AI Security Guard for AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/ai-aws-security-guard)

**Author:** luekrit kongkamon  
**Email:** luekrit.k@gmail.com

---

![Image](http://learn.nextwork.org/charmed_azure_silly_trout/uploads/ai-aws-security-guard_aws2m3n4o)

---

## Introducing Today's Project!

In this project, I’m building an AI-powered security guard for AWS that can detect and automatically fix cloud security misconfigurations.

I’m using Python with boto3 to scan AWS resources (starting with S3) and identify risky configurations such as public access exposure, missing security controls, and weak access settings.
Then, I’m integrating this scanner with AI-assisted workflows (Cursor + AWS MCP) so security issues can be remediated through natural-language instructions, similar to how modern security teams interact with automation and SOAR tools.

Instead of just flagging problems, this project focuses on closing the loop — detect ➝ assess severity ➝ fix ➝ verify.

### Key tools and concepts

Tools I used:

-AWS S3 for cloud storage and security configuration
-Python (boto3) to scan and interact with AWS resources programmatically
-Cursor + AWS MCP to apply AI-assisted remediation through natural language
-AWS CLI for verification and environment setup
-Git to manage and track project changes

Key concepts I learned include:

-Detecting cloud security misconfigurations using AWS-native controls
-Evaluating risk using severity-based findings rather than raw alerts
-Designing safe, idempotent remediation for cloud resources
-losing the loop with a detect → fix → verify security workflow
-Using AI as an operational interface, not just a coding assistant

The most important skill was:
Learning how to combine cloud security knowledge, automation, and AI to reduce real security risk in a practical and verifiable way.

### Challenges and wins

This project took me approximately 2 hours, with most of the time spent troubleshooting the initial environment configuration and ensuring that Python, AWS credentials, Cursor, and AWS MCP were all working together correctly.

The most challenging part was setting up the development environment and resolving issues related to virtual environments, paths, and tool integration. Once everything was configured properly, the actual implementation and testing progressed smoothly.

The most rewarding part was seeing the full workflow work end-to-end from creating a real cloud security issue, detecting it with my scanner, automatically fixing it using AI, and then verifying that the issue was fully resolved.

### Why I did this project

I did this project today to get hands-on experience with cloud security automation and AI-assisted remediation, and to reinforce how real security teams move from detection to action, not just reporting issues.

This project met my goals by allowing me to build and test a complete security workflow end-to-end — from creating a real AWS misconfiguration, detecting it with a custom scanner, fixing it automatically using AI, and verifying the fix with evidence. It helped me connect cloud security concepts with practical automation in a way that feels realistic and production-oriented.

Next, I plan to extend this approach to other AWS services, such as IAM or EC2, and continue exploring how AI can be used to safely automate security operations at scale.

---

## Setting Up Your Environment

In this step, I’m setting up my local environment so I can securely interact with AWS and automate security checks.

This involves installing Python and the AWS CLI, configuring my AWS credentials, and setting up the Python libraries needed to talk to AWS programmatically. I’m also creating a clean project structure so my scanning and remediation code stays organised as the project grows.

I need to do this first so I can safely scan AWS resources, collect security findings, and later automate fixes without running into authentication or tooling issues. This setup step ensures everything works end-to-end before I start building the actual AI security guard.

![Image](http://learn.nextwork.org/charmed_azure_silly_trout/uploads/ai-aws-security-guard_aws2c3d4e)

I verified my setup by running a series of commands inside the virtual environment to make sure everything was using the correct Python context.

First, I activated the virtual environment and checked which Python executable was being used. The output showed that Python was running from the venv directory inside my project, which confirmed the virtual environment was active.

Next, I checked the installed packages and verified that boto3 was available in the environment. The output showed boto3 listed with its version number, confirming that the AWS SDK for Python was installed correctly and accessible.

Finally, I verified that supporting tools like uv were installed and that the terminal prompt displayed (venv), which confirmed that all commands were running inside the isolated environment.

Together, these checks confirmed that my environment was correctly configured and ready to interact with AWS for security scanning and automation.

---

## Building the S3 Security Scanner

In this step, I’m building a Python-based security scanner that checks my AWS account for S3 public exposure misconfigurations.
I’m doing this so I can detect risky settings early, assign a severity level, and generate a clear report before moving to automated remediation with AI.

![Image](http://learn.nextwork.org/charmed_azure_silly_trout/uploads/ai-aws-security-guard_aws5f6g7h)

My scanner checks all S3 buckets in my AWS account to identify public access misconfigurations, which are one of the most common causes of cloud data exposure.

First, the scanner uses boto3 to connect to AWS and list all S3 buckets. For each bucket, it retrieves the Public Access Block configuration, which is AWS’s primary control for preventing public access to S3.

I then evaluate whether all four Public Access Block settings are enabled:

-BlockPublicAcls
-IgnorePublicAcls
-BlockPublicPolicy
-RestrictPublicBuckets

If any of these protections are disabled, the bucket is flagged as CRITICAL, because it means public access may still be possible through ACLs or bucket policies.

---

## Connecting Cursor to AWS with MCP

In this step, I’m connecting Cursor to my AWS account using the AWS Model Context Protocol (MCP). This involves configuring the MCP integration, restarting Cursor, and verifying that it can successfully access my AWS resources.

I need to do this so I can interact with my AWS environment through conversation, which allows me to inspect resources, analyze security findings from my scanner, and safely apply fixes using natural-language instructions instead of manual commands.

![Image](http://learn.nextwork.org/charmed_azure_silly_trout/uploads/ai-aws-security-guard_aws8i9j0k)

I connected Cursor to AWS by configuring the AWS Model Context Protocol (MCP) inside Cursor’s settings. This involved enabling the built-in AWS MCP server, which allows Cursor to securely access my AWS environment using my existing local AWS credentials.

Once enabled, Cursor gained context-aware access to my AWS account, allowing it to inspect resources and reason about infrastructure state rather than just generating code.

---

## Testing Your Scanner

In this step, I’m deliberately creating an insecure S3 bucket using Cursor + AWS MCP so I can validate that my security scanner actually works in a real AWS environment.

This involves using natural-language instructions in Cursor to introduce a controlled security misconfiguration, then running my Python scanner to confirm that the issue is detected and correctly classified.

I need to do this so I can prove end-to-end functionality:
AI-assisted infrastructure changes → automated detection → actionable security findings.

![Image](http://learn.nextwork.org/charmed_azure_silly_trout/uploads/ai-aws-security-guard_aws2m3n4o)

I tested my scanner by deliberately creating an insecure S3 test bucket using Cursor with AWS MCP. The test bucket was created without a Public Access Block configuration, which simulates a common real-world security misconfiguration that can lead to unintended public exposure.

After creating the test bucket, I ran my scanner using: "python scanner.py"

When the scan ran, it detected the test bucket and flagged it as CRITICAL, reporting that the bucket had no Public Access Block configured. The scanner completed successfully and reported a single security issue, which matched the expected outcome of the test.

This shows that my scanner is able to detect real AWS misconfigurations introduced through AI-driven changes, correctly assess their severity, and produce actionable security findings. It confirms that the full workflow — AI-created issue → automated detection → clear security alert — is working as intended.

---

## Fixing Security Issues with AI

In this part, I’m using AI to automatically fix the security issues that my scanner detected. Instead of manually logging into the AWS Console, I use Cursor with AWS MCP to apply security remediations through natural-language instructions.

This lets me close the security loop, moving from detection to remediation and verification, which is how real security operations teams work in practice.

Professional security tools do this because detection alone doesn’t reduce risk. Security findings only become valuable when they are fixed quickly, safely, and consistently. Automating remediation reduces human error, speeds up response time, and ensures security controls are applied in a repeatable way across cloud environments.

This step demonstrates how AI can act as a secure operational interface, helping scale cloud security efforts without increasing manual overhead.

![Image](http://learn.nextwork.org/charmed_azure_silly_trout/uploads/ai-aws-security-guard_awssec2b3c)

I fixed the security issues by using Cursor with AWS MCP to automatically apply AWS security best practices through natural-language instructions. The issue detected was a CRITICAL S3 misconfiguration, where a test bucket had no Public Access Block configured.

Cursor translated my instructions into AWS actions and enabled full S3 Block Public Access on the affected bucket. I then re-ran my scanner to confirm that the issue was resolved and that no security findings remained.

---

## Wrap-up

---

---
