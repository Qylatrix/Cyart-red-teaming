# Create folder structure
mkdir -p cyart-red-teaming/"Week 2"/Screenshots

# Go into the folder
cd cyart-red-teaming/"Week 2"

# Download the README I made (or copy-paste it manually)
# Then move your screenshots into Screenshots/ folder

# Go back to root
cd ..

# Initialize git
git init
git add .
git commit -m "Add Week 2 - Advanced Threat Analysis & Practical Tasks"
git branch -M main
git remote add origin https://github.com/Qylatrix/cyart-red-teaming.git
git push -u origin main
