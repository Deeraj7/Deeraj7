name: Update README

on:
  schedule:
    - cron: "0 0 * * *"  # Runs every day at midnight

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Update README with GitHub Repositories
        uses: actions/github-script@v4
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const repos = await github.repos.listForUser({
              username: 'Deeraj7', // Change to your GitHub username
              sort: 'updated',
              per_page: 5
            });
            const repoList = repos.data.map(repo => `- [${repo.name}](${repo.html_url}): ${repo.description}`).join('\n');
            const content = `
            # 👋 Hi, I’m Deeraj Thakkilapati

            ![Profile Views](https://komarev.com/ghpvc/?username=Deeraj7&color=blue)  
            I’m a data engineering enthusiast who loves turning ideas into real-world solutions. Whether it’s building data pipelines or finding innovative ways to make data more accessible, I’m always ready to dive in and create impact.

            ## 🔥 Latest Projects
            ${repoList}

            ## 📫 Connect with Me!

            [![Email Badge](https://img.shields.io/badge/Email-thakkilapatideeraj@gmail.com-red?style=flat-square&logo=gmail&logoColor=white)](mailto:thakkilapatideeraj@gmail.com)
            [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/deerajthakkilapati/)

            Let’s connect and explore how data can drive meaningful change!
            `;

            const base64Content = Buffer.from(content).toString('base64');
            await github.repos.createOrUpdateFileContents({
              owner: 'Deeraj7',
              repo: 'Deeraj7',
              path: 'README.md',
              message: 'Update README with latest projects',
              content: base64Content,
              committer: {
                name: 'GitHub Action',
                email: 'action@github.com'
              }
            });
