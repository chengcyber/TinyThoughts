# Read file content

```
(* Function to read a file line by line *)
let read_file file_name =
  let in_channel = open_in file_name in
  try
    while true do
      let line = input_line in_channel in
        print_endline line
    done
  with End_of_file ->
    close_in in_channel

let my_fun () =
  let f = "file.ml" in
    read_file f

;;

my_fun ()
```
