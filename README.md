# ugh
import numpy as np
from matplotlib import pyplot as pp
import scipy.optimize as optimize
import scipy.stats
import math

def fitFunction(x, k_B, T, r, b, f_0):
    return (k_B*T)/((np.power(r,2)*math.pi^2*b)*(np.power(x,2)+np.power((f_0),2)))
fig = pp.figure()
ax = fig.add_subplot(1,1,1)
x = np.loadtxt("Documents/OptTdata/01/yFreq.txt", comments="#", delimiter="\n", unpack=False)#np.loadtxt("OptTdata/01/Time.txt", comments="#", delimiter="\n", unpack=False) #-6*np.log(np.random.random(100000))
y = np.loadtxt("Documents/OptTdata/01/yPSD.txt", comments="#", delimiter="\n", unpack=False)
nBins=1000
#countsx,binx = np.histogram(x,nBins)
#countsy,biny = np.histogram(y,nBins)
ax.plot(x, y)

# fit to a gaussian function
fitErrors = np.sqrt(y+1)
initialGuess = []
fittedParameters, covariance = optimize.curve_fit(fitFunction, x, y, p0=initialGuess, sigma=fitErrors, maxfev=100000)
[fittedk_B, fittedT, fittedr, fittedb,fittedf_0] = fittedParameters

# put the fitted curve values into an array and plot them
fittedCurve = fitFunction(x, fittedk_B, fittedT, fittedr, fittedb, fittedf_0) 

ax.grid(True)

ax.plot(x, fittedCurve, 'r-', linewidth=1)
ax.set_yscale('log')
ax.set_xscale('log')
pp.show()


