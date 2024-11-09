---
title: "GITHUB: Actions to show latest Hashnode posts in Github"
datePublished: Wed Nov 29 2023 13:40:44 GMT+0000 (Coordinated Universal Time)
cuid: clpjtefww000v09l98czf6rs8
slug: github-actions-to-publish-hashnode-posts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701265406214/835faf19-729c-4eab-adf9-ac2cbb8eb341.png
tags: github, graphql, hashnode, actions, headlesshashnode

---

Hashnode introduced 'headless' and while having a look at it, It really seemed interesting. It's nice to have a way to play with our publications and posts from Hashnode, outside of Hashnode without tedious data scraping.

While reading another blog, which had some interesting use cases mentioned with some examples, one that talked about updating your static portfolio pages seemed really interesting.

I was really interested in hosting the Hashnode post in two places.

1. My [GitHub profile](https://github.com/anupammajhi) landing page.
    
2. My [Portfolio Website](https://anupamm.com/) (static), which is also [hosted via GitHub pages](https://github.com/anupammajhi/anupammajhi.github.io).
    

Since, both of them are static, and being able to update the latest few blog posts there dynamically would be cool.

So, the initial idea was simple: Write a Github action to run a script that fetches the posts from Hashnode, updates the html, and pushes to the repo. Do the same for the second one too. Too brute. So then I planned to convert them into Github Action steps instead.

## Action 1: Get Latest Hashnode Posts

This GitHub Action retrieves posts from a Hashnode publication using GraphQL. The action accepts certain inputs and outputs the resulting posts in JSON format.

### Inputs

* `HASHNODE_PUBLICATION_ID` (required): The ID of the Hashnode publication.
    
* `HASHNODE_GQL_ENDPOINT` (optional): The GraphQL endpoint for Hashnode. (default is [https://gql.hashnode.com](https://gql.hashnode.com/))
    
* `MAX_POSTS` (optional): Maximum number of posts to retrieve (default is 10).
    

### Outputs

* `result`: JSON string containing the retrieved posts.
    

### Example Usage

```yaml
name: Update Hashnode Posts
on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * *'

jobs:
  get_posts:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Get Hashnode Posts
        id: hashnode-posts
        uses: anupammajhi/githubaction-latest-hashnode-posts@v1.0.0
        with:
          HASHNODE_PUBLICATION_ID: ${{ secrets.HASHNODE_PUBLICATION_ID }}
          HASHNODE_GQL_ENDPOINT: 'https://gql.hashnode.com' # Optional
          MAX_POSTS: 8  # Optional
```

## Action 2: Update File in Repo with EJS

This GitHub Action allows you to update a file in your repository using an EJS template.

The action is designed to replace content in a specified file between designated start and end comments with the rendered output of an EJS template. It requires five inputs, including the file path, starting comment, ending comment, EJS template path, and the JSON input for the template.

### Inputs

#### `FILE_PATH` (required)

The relative path of the file in the repository that you want to update.

#### `STARTING_COMMENT` (required)

The starting comment in the file, marking the beginning of the content to be replaced.

#### `ENDING_COMMENT` (required)

The ending comment in the file, marking the end of the content to be replaced.

#### `EJS_TEMPLATE_PATH` (required)

The relative path of the EJS template file that will be used for rendering content.

#### `TEMPLATE_INPUT_JSON` (required)

The JSON input that the EJS template will use for variable extraction. Ideally, this should be obtained from another action step.

### Usage

```yaml
- uses: actions/checkout@v4
  with:
    # The relative path of the file in the repository that you want to update.
    FILE_PATH: ''

    # The starting comment in the file mentioned in FILE_PATH, marking the beginning of the content to be replaced.
    # This usually depends on which file you are trying to update. 
    # e.g. for html/markdown file you can use '<!-- YOUR_STARTING_COMMENT -->'
    # e.g. for js file you can use '// YOUR_STARTING_COMMENT'
    STARTING_COMMENT: ''

    # The ending comment in the file mentioned in FILE_PATH, marking the end of the content to be replaced.
    # This usually depends on which file you are trying to update. 
    # e.g. for html/markdown file you can use '<!-- YOUR_ENDING_COMMENT -->'
    # e.g. for js file you can use '// YOUR_ENDING_COMMENT'
    ENDING_COMMENT: ''

    # The relative path of the EJS template file that will be used for rendering content.
    # [Learn more about EJS templates](https://ejs.co/#docs)
    EJS_TEMPLATE_PATH: ''

    # The JSON input that the EJS template will use for variable extraction. 
    # Ideally, this should be obtained from another action step.
    TEMPLATE_INPUT_JSON: ''
```

### Example Usage

```yaml
name: Update Hashnode Posts
on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * *'

jobs:
  get_posts:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Get Hashnode Posts
        id: hashnode_posts
        uses: anupammajhi/githubaction-latest-hashnode-posts@v1.0.0
        with:
          HASHNODE_PUBLICATION_ID: ${{ secrets.HASHNODE_PUBLICATION_ID }}
          HASHNODE_GQL_ENDPOINT: 'https://gql.hashnode.com' # Optional
          MAX_POSTS: 8  # Optional

      - name: Update File
        id: update_file
        uses: anupammajhi/githubaction-update-repo-file@v1.0.1
        with:
          FILE_PATH: "README.md"
          STARTING_COMMENT: "<!-- CONTENT_AUTO_START -->"
          ENDING_COMMENT: "<!-- CONTENT_AUTO_END -->"
          EJS_TEMPLATE_PATH: "readmeTemplate.ejs"
          TEMPLATE_INPUT_JSON: ${{ steps.hashnode_posts.outputs.result }}
```

## Create EJS Template to format posts

This template is used to make sure that the data is published in our file in the consistent format that we want. EJS is a standard templating tool, and that has been used here. Here is an example.

```xml
<% 
let count = 0;
let i = 0;
while (count < 4 && i < 10){
    if(jsonData[i].coverImage){
        count++;

        let blogUrl = jsonData[i].url
        let imageUrl = jsonData[i].coverImage.url
        
        const replacementUrl = "tech.anupamm.com"
        const originalUrlSearchString = /anupammajhi\.hashnode\.dev/g

        blogUrl = blogUrl.replace(originalUrlSearchString, replacementUrl)
        imageUrl = imageUrl.replace(originalUrlSearchString, replacementUrl)

        let date = new Date(jsonData[i].publishedAt);
        let formattedDate = date.toLocaleString('en-US', {
            year: 'numeric',
            month: 'long',
            day: 'numeric'
        });
%>
<tr>
<td style="width:30%"><a href="<%= blogUrl %>"><img src="<%= imageUrl %>" width="500" height="auto" /></a></td>
<td>
<a href="<%= blogUrl %>"><b><%= jsonData[i].title %></b></a><br />
<code><%= formattedDate %></code><br />
<p><%= jsonData[i].brief %></p>
</td>
</a>
</tr>
<% 
    }
    i++;
} 
%>
```

> Make sure the file that we are trying to update has the appropriate Starting and Ending Comments in place.

That's it. Now we have static pages hosted in Github that are updated daily with the posts from Hashnode.