When running a script with some arguments that are going to function as switches for functionality such as -v for verbosity of the script, it's best to utilize *getopts*.
```bash
while getopts "vf:" opt; do
	case $opt in
		v)
			verbosity=1 ;;
		f)
			file=$OPTARG ;;
		*)
			echo "Invalid switch" ;;
	esac
done

shift $((OPTIND-1))
```

This is a boilerplate code that parses arguments from script execution and it expects two switches.
-v (for verbosity check)
-f (with additional argument)

"vf:" When listing expected switches, putting : after the switch indicates that additional argument.
In this case file=$OPTARG could be -f myfile.txt and it will be assigned accordingly as we stated in the file variable.

*) is used as a wildcard for any other unexpected switch.