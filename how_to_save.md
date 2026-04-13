1. Save progress after each model
python
def save_progress(i):
    with open("progress.json", "w") as f:
        json.dump({"last_model": i}, f)
Inside your loop:

python
save_progress(i)
2. Load progress when restarting
At the top of your notebook:

python
import os, json

if os.path.exists("progress.json"):
    with open("progress.json", "r") as f:
        start_from = json.load(f)["last_model"] + 1
else:
    start_from = 0

print("Resuming from model:", start_from)
3. Resume training
python
for i in range(start_from, 50000):
    model = build_model().to(device)
    metrics = train_one_model(model, train_loader)

    save_model(model, i, metrics)
    save_progress(i)
