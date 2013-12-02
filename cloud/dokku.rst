Running manage.py commands
===================================
This is the only hacky part of the process. From inside your Droplet, lets a terminal session inside our container

::

    docker images # See a listing of all of your docker images
    docker run  -i -t app/myapp /bin/bash # Start an interactive terminal inside of your container

Now we need to bootstrap our terminal session with all of the correct paths and environment variables.

::

    export HOME=/app
    for file in /app/.profile.d/*; do source $file; done
    hash -r
    cd /app # This is the folder where django app got deployed

Now you can run your manage.py commands

::

    python manage.py syncdb
