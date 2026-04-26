# Setting up your Workspace (.venv)

To run our notebooks and scripts, you need a few specific Python libraries. To keep things clean and avoid mess with your other projects, we use a **Virtual Environment (.venv)**. Here's how to set it up:

## 1. Create the Environment
Open your terminal inside the project folder and run:
```bash
python3 -m venv .venv
```
*(This creates a little "bubble" where all our project tools will live.)*

## 2. Turn it on (Activate)
Depending on what computer you're using, the command is slightly different:

*   **Mac or Linux:**
    ```bash
    source .venv/bin/activate
    ```
*   **Windows:**
    ```bash
    .venv\Scripts\activate
    ```
*(You'll know it worked if you see `(.venv)` appear at the start of your command line!)*

## 3. Install the Tools
Now, just run this to install all the libraries we used (like Pandas and Seaborn):
```bash
pip install -r requirements.txt
```

---

### Pro Tip:
If you're using **VS Code**, it might ask you to select a "Kernel" for the notebooks. Make sure you pick the one that says `.venv` so it can find the tools you just installed!

Happy coding! 🚀
