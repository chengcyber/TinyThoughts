# Padding-bottom trick

set a 0 height container with particular padding-bottom

```
.img-container {
    padding-bottom: 56.25%; /* 16:9 ratio */
    height: 0;
    background-color: black;
    overflow: hidden;
}
```

position the image absolutely inside the container

```
.img-container img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
```

