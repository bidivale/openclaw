Your setup is solid. No need to change anything.
Here's the full picture:
Resource
Total
Used
Status
RAM
31 GiB
11 GiB
✅ 20 GiB available
Swap
32 GiB
0 B
✅ Untouched, ready as safety net
Disk (/)
937 GB
323 GB (37%)
✅ 567 GB free
Build cache
0 B
—
✅ Just cleared
Docker images
~27.9 GB
—
✅ Normal

Why Docker crashed 5 months ago: Large image pulls spiked RAM, 8 GB swap wasn't enough → OOM killed Docker. Your 32 GB swap fix was the right call.
Right now:
You're at 37% disk — very comfortable
Swap is 0% used — your RAM is fine
Docker images cleaned up today saved ~5+ GB
You do NOT need to enhance anything. The only scenario to watch for is if your images grow significantly (e.g. gliner2-gliner2-api at 7.73GB and llm-graph-builder-v3-backend at 4.75GB are your biggest). As long as you stay under ~850GB disk used you're safe.
One thing missing from your previous fix — Step 2 was never applied (max-concurrent-downloads: 1). Since you said you never pull multiple images at once, you're fine without it. But if you ever plan to pull several large images simultaneously, that's the safety valve to add.

