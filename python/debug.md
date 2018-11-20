import ptvsd

ptvsd.enable_attach(address=("0.0.0.0", 9999), redirect_output=True)
ptvsd.wait_for_attach()

