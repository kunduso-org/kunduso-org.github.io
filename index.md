---
layout: default
title: Home
---

<img class="alignnone size-full wp-image-1822" src="https://skdevops.files.wordpress.com/2022/06/aboutme-2022-6.png" alt="AboutMe-2022-6" width="925" height="335" />

I discuss the technical aspects of mastering DevOps principles via the **100+ posts I've written here.** I achieve that by writing regularly on CI/CD pipelines, code repositories, infrastructure as code, cloud deployment using tools and cloud platforms like Git/GitHub, Terraform/Terragrunt, PowerShell, Azure DevOps, AWS CLI, Azure CLI, Azure, and AWS cloud.

I have multiple certifications from AWS, HashiCorp, Microsoft, and GitHub ([check my Credly profile](https://credly.com/users/kunduso/badges)). In 2024, 2023, and 2022, I was awarded the **HashiCorp Ambassador award** for my blog articles and code snippets on GitHub on Terraform.

<img class="alignnone size-full wp-image-3688" src="https://skdevops.files.wordpress.com/2024/04/2024-04-certifications.png" alt="2024-04-Certifications" width="1024" height="540" />

*Please note: Earlier in 2023, Microsoft introduced a new feature to the Microsoft Learn profile to provide learners with flexible options for viewing and sharing their Microsoft certifications directly from Microsoft Learn. Hence, Credly won't have the latest information on the Microsoft Certification badges once they expire. You can read about that at [cred-share-validate](https://learn.microsoft.com/en-us/credentials/certifications/cred-share-validate). My Microsoft Learn profile is available at [Sourav Kundu Microsoft Learn](https://learn.microsoft.com/en-us/users/souravkundu-3010/transcript/dr96aek36q1mzpd).*

I have two decades of experience using software engineering tools and over a decade of experience using *DevOps principles and tools.* A few posts from my blog have also been featured in Microsoft Dev blogs.

## Why this blog?

Earlier in my career, I struggled technically. And I knew I wasn't the first one stumbling over the challenge. Still, unfortunately, insufficient technical use cases were available on the internet. They were either detailed courses on a product or a skill (Udemy, Pluralsight) or direct answers (StackOverflow). Don't get me wrong, these were lifesavers back then. However, I was looking for something in the middle - I did not have the time or desire to go through the entire product. At the same time, the tiny code snippets did not satisfy my curiosity.

> *I wanted a technical article that approached the challenge via a use case - discusses the challenge, discusses the approach, demonstrates the approach, and provides artifacts (link to [GitHub repository](https://github.com/kunduso)). And that is what you can expect from this blog.*

I categorized these articles under separate menu items so that you can easily search for them on AWS, Terraform, and Azure DevOps. You can also use the search button.

If you've come this far, thanks for reading and for your curiosity. If you've searched for a particular technical use case and have not found it, please do not hesitate to contact me using the **[Contact page](/contact/)**. I might not have written about a particular use case, but maybe I've encountered one and can help you.

Are you on LinkedIn? Let's connect. Please **[click here](https://www.linkedin.com/in/sourav12kundu/)** and drop in a message that you found me at skundunotes.com. I also love reading books and sometimes share about them on **[Twitter](https://twitter.com/isouravkundu)**.

---

## Latest Posts

{% for post in site.posts limit:10 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}

[View all posts →](/posts/)