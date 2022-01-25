# Push to a Different Repo



**Given**

- Cloned repo
- Want to fetch updates from this repo over time



**When**



**Then**

- Update the original one to 'upstream' then add `origin` to your own repo url

  ```
  $ git remote rename origin upstream
  $ git remote add origin git@github.com:YOU/YOUR_REPO
  ```

  