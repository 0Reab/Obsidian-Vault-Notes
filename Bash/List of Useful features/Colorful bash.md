Your bash output can be customized with colors and style such as bold and intensity.
Using either printf or echo -e we can utilize the color codes.
Keep in mind the color code will be persistent until you reset it with reset code.

```bash
red_color="\033[0;31m"
reset_color="\033[0m"

printf "${red_color}This text is red"
echo -e "\033[0;32mThis text is green${reset_color}"
```