# Agent File Uploads Guide

# 1.0 Summary

You can upload documents to the Workday Chat, and have your agents process them. To upload documents, click on the **Sources** button:  

<img width="698" height="212" alt="image1" src="https://github.com/user-attachments/assets/71594711-4bfe-4d95-819a-60b50fffb94a" />

And then just specify your file and ask the agent to process it. You can also ask that it download the file into the agent’s sandbox for further processing. A maximum of 10 file attachments can be included per message. For system file type limits on attachments, you can go to **Edit Tenant Setup \- System**. This setup page will display the maximum attachment size, image size,and allowed file types.

<img width="2048" height="458" alt="image2" src="https://github.com/user-attachments/assets/b1aadefb-cb69-4dfb-9e0d-abdcc94d151f" />

# 2.0 Gotchas/Manual Configuration

This functionality requires a manual configuration step. You will need to add the following resource in code mode:

```
tool-wids:
  - name: fileUploadTokenExchange # or similar user-friendly name, can be anything
    wid: 94fadfecb1bb100031d911f8ed0a0000
```

*Important Note:* The Developer Agent is unaware of this specific resource, so you will need to add it manually. This resource allows your agent to access documents on your behalf. If you do not add it, then your agent will fail to download any documents you upload.  
