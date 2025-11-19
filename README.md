# Trabalho final - Series Temporais

## Aos contribuintes

Crie o arquivo `.git/hooks/pre-commit` com o seguinte script para automaticamente compilar o .Rmd para
HTML ao commitar.

```bash
#!/bin/sh

# define the Rmd file
RMD_FILE="src/trabalho.Rmd"
OUTPUT_FILE="AnaliseDeCrimesSP.html"


# check if the Rmd file is being committed
if git diff --cached --name-only | grep -q "$RMD_FILE"; then
    echo "Rendering $RMD_FILE to HTML..."
    
    # Render using Rscript
    Rscript -e "rmarkdown::render('$RMD_FILE', output_file = '$OUTPUT_FILE')"
    
    # Check if render succeeded
    if [ $? -eq 0 ]; then
        echo "Render successful. Adding HTML to commit."
        git add "src/$OUTPUT_FILE"
    else
        echo "Error: Rendering failed. Commit aborted."
        exit 1
    fi
fi
```