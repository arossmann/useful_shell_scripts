# With Docker
With docker, pseudo-tty allocation is deactivated by default, but the -i (interactive) option is required with the restore command.

## mongodump
No Auth : 
```docker exec <mongodb container> sh -c 'mongodump --archive' > db.dump```

Authenticated : 
```docker exec <mongodb container> sh -c 'mongodump --authenticationDatabase admin -u <user> -p <password> --db <database> --archive' > db.dump```

## mongorestore
No Auth : 
```docker exec -i <mongodb container> sh -c 'mongorestore --archive' < db.dump```

Authenticated : 
```docker exec -i <mongodb container> sh -c 'mongorestore --authenticationDatabase admin -u <user> -p <password> --db <database> --archive' < db.dump```

# With Docker Compose
With docker-compose, pseudo-tty allocation needs to be deactivated explicitly each time with -T :

## mongodump
```docker-compose exec -T <mongodb service> sh -c 'mongodump --archive' > db.dump```

## mongorestore
```docker-compose exec -T <mongodb service> sh -c 'mongorestore --archive' < db.dump```

