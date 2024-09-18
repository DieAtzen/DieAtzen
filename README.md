const fs = require('fs');
const axios = require('axios');

const GITHUB_ORG_NAME = 'DieAtzen';
const GITHUB_API_URL = `https://api.github.com/orgs/${GITHUB_ORG_NAME}`;

async function fetchGitHubData() {
    const { data } = await axios.get(GITHUB_API_URL);
    return {
        orgName: data.login,
        orgDescription: data.description || 'No description available.',
        publicRepos: data.public_repos,
        followers: data.followers,
        membersUrl: data.members_url.replace('{/member}', ''), // Get the members URL without the placeholder
        avatarUrl: data.avatar_url,
        htmlUrl: data.html_url,
    };
}

async function generateReadme() {
    const githubData = await fetchGitHubData();
    
    const readmeTemplate = `
# 👋 Welcome to ${githubData.orgName}!

${githubData.orgDescription}

## 🔧 Technologies & Languages

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Java](https://img.shields.io/badge/Java-007396?style=flat&logo=java&logoColor=white)

## 📊 GitHub Stats

![${githubData.orgName}'s GitHub Stats](https://github-readme-stats.vercel.app/api?username=${githubData.orgName}&show_icons=true&theme=radical)

## 🔗 Our Repositories

- [Link to our GitHub](https://github.com/${githubData.orgName})

## 📬 Connect with Us
- You can find more information and connect with our members [here](${githubData.membersUrl}).

---

## 📊 Additional GitHub Statistics

- 🌟 **Public Repositories:** ${githubData.publicRepos}  
- 👥 **Followers:** ${githubData.followers}  
- 🔗 **GitHub Profile:** [Visit Our Profile](https://github.com/${githubData.orgName})
    `;

    fs.writeFileSync('README.md', readmeTemplate.trim());
    console.log('README.md has been updated.');
}

generateReadme().catch(error => {
    console.error('Error generating README:', error);
});
