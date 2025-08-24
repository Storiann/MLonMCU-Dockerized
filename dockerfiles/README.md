

# ML on MCU — Docker + Jupyter Quick Start

This guide shows how to set up a clean Docker environment to run Jupyter (Notebook/Lab) for the **MLonMCU** exercises.


---

## 1) Clone the Repository


The structure should look like this:

```
MLonMCU/
├─ ex1
├─ ex2
├─ ...
└─ dockerfiles/
   ├─ Dockerfile
   ├─ docker-compose.yml
   ├─ requirements.txt
   ├─ start.sh
   └─ README.md
```

---

## 2) Install Docker (If you do not have it)

1. Download and install **Docker Desktop** for your OS:

   * Windows / macOS: install Docker Desktop and complete the setup.
   * Linux: install Docker Engine and Docker Compose v2 via your distro’s package manager.

2. Start Docker Desktop (or the Docker daemon on Linux).
   You should see an “Engine running” / “Docker is running” indicator in the app.

3. Verify in a terminal:

   ```bash
   docker --version
   ```

---

## 3) Build the Image

From inside the `dockerfiles` directory:

```bash
cd MLonMCU/dockerfiles
docker compose build
```

This step downloads dependencies and may take a while the first time. It should finish without errors.

---

## 4) Run Jupyter

Start the container and launch Jupyter:

```bash
docker compose up
```

When it starts, the terminal will print a URL that looks like:

```
http://127.0.0.1:8888/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Open **that** URL in your browser, or open:

```
http://localhost:8888
```

…and paste the displayed token if Jupyter asks for it.


---

## 5) Working with the `workspace` Folder

Inside Jupyter, you’ll see a folder named **`workspace`**. This is the working directory for your notebooks and files.

You have two options:

* **Option A:** Keep the exercises where they are and just open notebooks from there.
* **Option B (recommended for simplicity):** Move the exercise folders into `workspace`:

  From **inside** `MLonMCU/dockerfiles` (host terminal):

  ```bash
  mv ../ex* workspace
  ```

  Then refresh Jupyter: the exercise folders will appear under `workspace/`.

---

## 6) Stopping and Restarting

* Stop (in the same terminal): press **Ctrl + C**.
* Cleanly shut down:

  ```bash
  docker compose down
  ```
* Start again later:

  ```bash
  docker compose up
  ```

---

## 7) Troubleshooting & Tips

* **Port already in use (8888):**
  Change the left side of the port mapping in `docker-compose.yml` (e.g., `8890:8888`), then:

  ```bash
  docker compose down
  docker compose up
  ```
* **Cannot see `workspace` updates:**
  Refresh the Jupyter page. If you moved files on the host while Jupyter was open, a refresh helps.
* **Permissions issues on Linux/macOS:**
  The compose file usually maps your host user ID automatically. If files appear as root-owned, rebuild with your UID or adjust the compose `user`/`args` settings as needed. For permission issues, please try `sudo`.
* **Rebuild after changing requirements:**

  ```bash
  docker compose build --no-cache
  docker compose up --force-recreate
  ```


---

## Per-Exercise Quick Test Notes

This section summarizes the minimal actions to quickly verify each exercise inside the provided Docker + Jupyter environment.

### Global notes

1. **Exercise 8 is not ready yet** — it will require **PyTorch** and will be added in the **next commits**.
2. Some notebooks start with cells like `!pip install ...`. **Delete those cells** because all required packages are already installed in the Docker image. (They will be removed in the next commit)

### Per-exercise checklist (to speed up your testing)

* **ex1** — No Jupyter file.
* **ex2** — Set the `audio_path` variable (point it to your local audio file).
* **ex3** — `Ex3_3` may fail or throw an error. The fix for this issue is ready and will be pushed together with the Docker integration to the MLonMCU repo.
* **ex4** — No Jupyter file.
* **ex5** — Download the dataset from the link in **`main.pdf`**. For a quick test, in **Cell 6** set the **compute compression ratio** to **10, 10**. (This won’t give correct results; it’s only to test the script.)
* **ex6** — During training you’ll need to **re-run cells** (as noted in the notebook). For faster testing, set **epochs = 1** and run through quickly.
* **ex7** — Saving model visualizations currently raises an error. The fix for this issue is ready and will be pushed together with the Docker integration to the MLonMCU repo.
* **ex8** — Not ready yet; requires **PyTorch**. Will arrive in the next commits.




