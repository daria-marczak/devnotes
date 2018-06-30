# Notes from How git works course from Paolo Perrotta

The basic idea in git is a simple map: it maps keys to values. It's persistent: stored on the disc. Values are the sequences of bytes that has a calculated key with SHA1hash. 

echo "Apple Pie" | git hash-object --stdin command is going to tell git that this is a content, not to make git search for a file "Apple Pie". The output is: 23991897e13e47ed0adb91a0082c31c82fe0cbe5

SHA1s are unique.