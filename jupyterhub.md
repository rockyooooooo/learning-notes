## JupyterHub

### Install

1. install conda or pip(I use conda)
2. install jupyterhub

```bash
conda install -c conda-forge jupyterhub
```

1. install notebook

```bash
conda install notebook
```

1. start the hub server

```bash
jupyterhub
```

1. go `http://localhost:8000` on browser
2. enter your machine’s credential
3. done

Multiple user → [https://github.com/jupyterhub/jupyterhub/wiki/Using-sudo-to-run-JupyterHub-without-root-privileges](https://github.com/jupyterhub/jupyterhub/wiki/Using-sudo-to-run-JupyterHub-without-root-privileges)
