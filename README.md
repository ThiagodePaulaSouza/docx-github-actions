# docx-github-actions
Control your docx versioning with github.

`.github/Workflows/main.yml`
```yml
name: Convert docx to md
on: push
jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master

      - name: install dependencies
        run: sudo apt install pandoc

      - name: Convert docx to md using pandoc
        run: for i in $(find . -type f -name "*.docx" | sed 's/\.\///g' | sed 's/\.docx//g'); do pandoc -f docx -t markdown $i.docx -o $i.markdown;done
        
      - name: Commit changes
        run: |
          git config --global user.name '[your-user-name]'
          git config --global user.email '[your-email]'
          git remote set-url origin https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}
          git status
          git add .
          git commit -m "Update Docx - $(git log -1 --pretty=%B)"
          git push -u origin main
```
