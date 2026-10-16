# Claude Code

```bash
claude mcp add --transport http aiseogic https://app.aiseogic.com/mcp
```

Start Claude Code, run `/mcp`, select **aiseogic** and choose **Authenticate**. Your browser opens the AISEOgic sign-in and consent page; click **Allow**.

Share the connection with your team by committing a project-scoped config:

```bash
claude mcp add --transport http --scope project aiseogic https://app.aiseogic.com/mcp
```

Each person still signs in with their own AISEOgic account.

Example task, run inside your website's project folder:

> Audit https://your-site.com with AISEOgic, then fix the top issues in this project. Show me the changes before you apply them.
