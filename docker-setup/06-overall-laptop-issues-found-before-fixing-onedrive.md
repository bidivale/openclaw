Chrome renderer spike on open

Chrome renderer process  →  51% CPU at the moment
This is normal for a few seconds when Chrome first opens a heavy page, but on your CPU it causes the whole machine to feel frozen because you only have 4 physical cores.

The real root cause: Your CPU
Your machine is a Dell Latitude 7390 with an i7-8650U — a 2018 mobile chip with only 4 physical cores. 32GB RAM is fine, but CPU is the bottleneck. When Chrome + VS Code + OneDrive all spike at the same time (especially at boot), all 4 cores get saturated and everything hangs.

