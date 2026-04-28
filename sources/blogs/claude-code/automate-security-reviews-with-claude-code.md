# Automate security reviews with Claude Code | Claude

## Automate security reviews with Claude Code

-

Category

Product announcements

-

Product

Claude Code

-

Date

August 6, 2025

-

Reading time

5

min

-

Share
Copy link
https://claude.com/blog/automate-security-reviews-with-claude-code

Today we're introducing automated security reviews in Claude Code. Using our GitHub Actions integration and a new /security-review command, developers can easily ask Claude to identify security concerns—and then have it fix them.

As developers increasingly rely on AI to ship faster and build more complex systems, ensuring code security becomes even more critical. These new features let you integrate security reviews into your existing workflows, helping you catch vulnerabilities before they reach production.

#### Review code for vulnerabilities from your terminal

The new /security-review command lets you run ad-hoc security analyses from your terminal before committing code. Run the command in Claude Code, and Claude will search your codebase for potential vulnerabilities and provide detailed explanations of any issues found.

This command uses a specialized security-focused prompt that checks for common vulnerability patterns including:

- SQL injection risks
- Cross-site scripting (XSS) vulnerabilities
- Authentication and authorization flaws
- Insecure data handling
- Dependency vulnerabilities

You can also ask Claude Code to implement fixes for each issue after they’re identified. This keeps security reviews in your inner development loop, catching issues early when they're easiest to fix.

#### Automate security reviews for new pull requests

The new GitHub action for Claude Code takes security reviews a step further by automatically analyzing every pull request when it's opened. When configured, the action:

- Triggers automatically on new pull requests
- Reviews code changes for security vulnerabilities
- Applies customizable rules to filter out false-positives and known issues
- Posts comments inline on the PR with any concerns found, including recommendations for fixes

This creates a consistent security review process across your entire team, ensuring no code reaches production without a baseline security review. The action integrates with your existing CI/CD pipeline and can be customized to match your team's security policies.

#### Improving product security at Anthropic

We're using these features ourselves to help secure the code our team ships to production, including Claude Code itself. Since setting up the GitHub action, this has already caught security vulnerabilities in our own code and prevented them from being shipped.

For example, last week, our team built a new feature for an internal tool that relied on starting a local HTTP server meant to accept local connections. The GitHub action identified a remote code execution vulnerability exploitable through DNS rebinding and it was fixed before the PR was ever merged.

In another case, an engineer built a proxy system to enable secure management of internal credentials. The GitHub action automatically flagged that this proxy was vulnerable to SSRF attacks, and we promptly fixed this issue.

#### Getting started

Both features are available now for all Claude Code users. To start using automated security reviews:

- For the /security-review command: Simply update Claude Code to the latest version and run /security-review in your project directory. See the documentation to customize your own version of the command
- For the GitHub action: See the documentation for step-by-step installation and configuration instructions

No items found.

PrevPrev

0/5

NextNext

eBook

###

FAQ

No items found.

### Related posts

Explore more product news and best practices for teams building with Claude.

Apr 9, 2026

#### The advisor strategy: Give agents an intelligence boost

Product announcements

The advisor strategy: Give agents an intelligence boostThe advisor strategy: Give agents an intelligence boost

The advisor strategy: Give agents an intelligence boostThe advisor strategy: Give agents an intelligence boost

Jun 25, 2025

#### Turn ideas into interactive AI-powered apps

Product announcements

Turn ideas into interactive AI-powered appsTurn ideas into interactive AI-powered apps

Turn ideas into interactive AI-powered appsTurn ideas into interactive AI-powered apps

Apr 9, 2026

#### Making Claude Cowork ready for enterprise

Product announcements

Making Claude Cowork ready for enterpriseMaking Claude Cowork ready for enterprise

Making Claude Cowork ready for enterpriseMaking Claude Cowork ready for enterprise

Apr 8, 2026

#### Claude Managed Agents: get to production 10x faster

Product announcements

Claude Managed Agents: get to production 10x fasterClaude Managed Agents: get to production 10x faster

Claude Managed Agents: get to production 10x fasterClaude Managed Agents: get to production 10x faster

### Transform how your organization operates with Claude

See pricing

See pricingSee pricing

Contact sales

Contact salesContact sales

Get the developer newsletter

Product updates, how-tos, community spotlights, and more. Delivered monthly to your inbox.

SubscribeSubscribe

Please provide your email address if you'd like to receive our monthly developer newsletter. You can unsubscribe at any time.

Thank you! You’re subscribed.

Sorry, there was a problem with your submission, please try again later.
