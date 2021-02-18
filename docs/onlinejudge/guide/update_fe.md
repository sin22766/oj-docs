# Modify the front end

Refer to https://github.com/QingdaoU/OnlineJudgefe for development and construction

Then `npm run build` can get a dist folder, the file structure is as follows

```
➜ OnlineJudgeFE git:(master) ✗ tree dist
dist
├── admin
│ └── index.html
├── index.html
└── static
    ├── css
    │ ├── admin.127f3da5b09451926728de2829ebb32e.css
    │ ├── loader.css
    │ ├── oj.0ba722f43ddbeb758cde2f9dc804455e.css
    │ └── vendor.f033d6c4c74b6b40e92ca86f168fd0be.css
    ├── fonts
    │ ├── KaTeX_AMS-Regular.3d8245d.woff2
    │ ├── KaTeX_AMS-Regular.ac1d46d.woff

....
....

```

Copy the `dist` folder to a directory on the server, such as `/data/OnlineJudgeDeploy/data/backend/dist`, then modify `docker-compose.yml`, `volumes in the `oj-backend` module Add a line `- /data/OnlineJudgeDeploy/data/backend/dist:/app/dist` in the `(please modify the actual path before the colon), and then `docker-compose up -d`.

Note that this modification method will overwrite the front-end files in the container. When a new version of the front-end is released in the future, please use the same method to update.

Video demo: https://www.bilibili.com/video/av37051523/
