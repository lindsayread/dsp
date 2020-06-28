[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Examples and Exercises from Think Stats, 2nd Edition\n",
    "\n",
    "http://thinkstats2.com\n",
    "\n",
    "Copyright 2016 Allen B. Downey\n",
    "\n",
    "MIT License: https://opensource.org/licenses/MIT\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "from __future__ import print_function, division\n",
    "\n",
    "%matplotlib inline\n",
    "\n",
    "import numpy as np\n",
    "\n",
    "import nsfg\n",
    "import first"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Given a list of values, there are several ways to count the frequency of each value."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "t = [1, 2, 2, 3, 5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "You can use a Python dictionary:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{1: 1, 2: 2, 3: 1, 5: 1}"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "hist = {}\n",
    "for x in t:\n",
    "    hist[x] = hist.get(x, 0) + 1\n",
    "    \n",
    "hist"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "You can use a `Counter` (which is a dictionary with additional methods):"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Counter({1: 1, 2: 2, 3: 1, 5: 1})"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "from collections import Counter\n",
    "counter = Counter(t)\n",
    "counter"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Or you can use the `Hist` object provided by `thinkstats2`:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Hist({1: 1, 2: 2, 3: 1, 5: 1})"
      ]
     },
     "execution_count": 44,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import thinkstats2\n",
    "hist = thinkstats2.Hist([1, 2, 2, 3, 5])\n",
    "hist"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "`Hist` provides `Freq`, which looks up the frequency of a value."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2"
      ]
     },
     "execution_count": 45,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "hist.Freq(2)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "You can also use the bracket operator, which does the same thing."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2"
      ]
     },
     "execution_count": 46,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "hist[2]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If the value does not appear, it has frequency 0."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "hist[4]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The `Values` method returns the values:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "dict_keys([1, 2, 3, 5])"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "hist.Values()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "So you can iterate the values and their frequencies like this:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "1 1\n",
      "2 2\n",
      "3 1\n",
      "5 1\n"
     ]
    }
   ],
   "source": [
    "for val in sorted(hist.Values()):\n",
    "    print(val, hist[val])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Or you can use the `Items` method:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "1 1\n",
      "2 2\n",
      "3 1\n",
      "5 1\n"
     ]
    }
   ],
   "source": [
    "for val, freq in hist.Items():\n",
    "     print(val, freq)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "`thinkplot` is a wrapper for `matplotlib` that provides functions that work with the objects in `thinkstats2`.\n",
    "\n",
    "For example `Hist` plots the values and their frequencies as a bar graph.\n",
    "\n",
    "`Config` takes parameters that label the x and y axes, among other things."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import thinkplot\n",
    "thinkplot.Hist(hist)\n",
    "thinkplot.Config(xlabel='value', ylabel='frequency')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "As an example, I'll replicate some of the figures from the book.\n",
    "\n",
    "First, I'll load the data from the pregnancy file and select the records for live births."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [],
   "source": [
    "preg = nsfg.ReadFemPreg()\n",
    "live = preg[preg.outcome == 1]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Here's the histogram of birth weights in pounds.  Notice that `Hist` works with anything iterable, including a Pandas Series.  The `label` attribute appears in the legend when you plot the `Hist`. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "hist = thinkstats2.Hist(live.birthwgt_lb, label='birthwgt_lb')\n",
    "thinkplot.Hist(hist)\n",
    "thinkplot.Config(xlabel='Birth weight (pounds)', ylabel='Count')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Before plotting the ages, I'll apply `floor` to round down:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [],
   "source": [
    "ages = np.floor(live.agepreg)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAYUAAAEGCAYAAACKB4k+AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjMsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+AADFEAAAX2ElEQVR4nO3df7DddX3n8edbCCQgEgIXFhLYpGtAUSHoTYzF0ZA4EUmnCSMRxWJ0ssaO0CJ2rMiuU6x0xI5bLKsDjUAJSAUKsqGKthCIzjKEmAClSFBSF8JdWJKGnwpRE977x/ncr4fk/jgh93vOudznY+bO+X4/53POed/vJOd1P5/vr8hMJEkCeF2nC5AkdQ9DQZJUMRQkSRVDQZJUMRQkSZW9O13AnjjkkENy6tSpnS5DkkaV9evX/0dm9gz03KgOhalTp7Ju3bpOlyFJo0pEPDbYc04fSZIqhoIkqWIoSJIqo3qfgqSx47e//S19fX1s27at06WMGuPHj2fKlCmMGzeu5dcYCpJGhb6+Pg444ACmTp1KRHS6nK6XmWzdupW+vj6mTZvW8uucPpI0Kmzbto2DDz7YQGhRRHDwwQfv9sjKUJA0ahgIu+fVbC9DQZJUcZ+CpFHp7Au/M6Lv943//pERfb/RylBQRwz3H9r/oBrrduzYwV577dX2z3X6SJJ2w6JFi3jHO97BW97yFpYvXw7AFVdcwdFHH82cOXP45Cc/ydlnnw3Ali1b+OAHP8jMmTOZOXMmd911FwAXXHABZ555JnPnzmX69Ol861vfAmD16tWcdNJJnHHGGbztbW8D4Nvf/jazZs1ixowZfOpTn2LHjh1DfuaecqSgWgw1EnAUoNHsyiuvZNKkSbz00kvMnDmTBQsW8OUvf5l7772XAw44gLlz53L88ccDcM4553Duuefy7ne/m02bNvH+97+fDRs2APDAAw+wZs0afvWrX3HCCSewYMECANauXcuDDz7ItGnT2LBhA9dffz133XUX48aN49Of/jTXXnst73vf+wb9zD1lKEjSbrjkkku4+eabAXj88ce55ppreO9738ukSZMAWLx4MT//+c8BuP3223nooYeq1z7//PO88MILACxcuJAJEyYwYcIETjrpJNauXcvEiROZNWtWdV7BqlWrWL9+PTNnzgTgpZde4tBDD2Xt2rWDfuaeMhQkqUWrV6/m9ttv5+6772a//fZjzpw5HHPMMdVf/zt7+eWXufvuu5kwYcIuz+18uGj/+v7771+1ZSZLlizhK1/5yiv69odSHdynIEkteu655zjooIPYb7/9ePjhh1mzZg0vvvgiP/rRj3jmmWfYvn07N910U9V//vz5fOMb36jW77///mp55cqVbNu2ja1bt7J69epqNNBs3rx53HjjjWzevBmAp59+mscee4xZs2YN+pl7ypGCpFGpE/umTj75ZC677DKOO+44jjnmGGbPns3kyZM5//zzeec738kRRxzBsccey4EHHgg0pprOOussjjvuOLZv38573vMeLrvsMgBmzZrFggUL2LRpE1/84hc54ogjdpkCOvbYY7nwwguZP38+L7/8MuPGjeOb3/wms2fPHvQz95ShIEkt2nffffnBD36wS3tvby/Lli1j+/btnHrqqcyfPx+AQw45hOuvv37A9zr66KOro5f6zZkzhzlz5ryi7fTTT+f000/f5fVnnHHGgJ+5p5w+kqQ9dMEFFzBjxgze+ta3Mm3aNBYtWjRqP9ORgiTtoa997Wu71f+CCy5o+2e2ypGCpFEjMztdwqjyarZXraEQERMj4saIeDgiNkTEuyJiUkTcFhGPlMeDSt+IiEsiYmNEPBARb6+zNkmjy/jx49m6davB0KL++ymMHz9+t15X9/TR3wI/zMzTImIfYD/gfGBVZl4UEecB5wGfBz4ATC8/7wQuLY+SxJQpU+jr62PLli2dLmXU6L/z2u6oLRQi4g3Ae4CPA2Tmb4DfRMRCYE7ptgJYTSMUFgJXZ+PPgDVllHF4Zj5ZV42SRo9x48bt1h3E9OrUOX30e8AW4O8j4r6IuDwi9gcO6/+iL4+Hlv6TgcebXt9X2l4hIpZFxLqIWOdfDJI0suoMhb2BtwOXZuYJwK9oTBUNZqBbBO0yeZiZyzOzNzN7e3p6RqZSSRJQbyj0AX2ZeU9Zv5FGSDwVEYcDlMfNTf2PbHr9FOCJGuuTJO2ktn0Kmfn/IuLxiDgmM38GzAMeKj9LgIvK48rykluAsyPiOho7mJ9zf4KG4816pJFV99FHfwJcW448+gXwCRqjkxsiYimwCVhc+t4KnAJsBF4sfSVJbVRrKGTm/UDvAE/NG6BvAmfVWY8kaWie0SxJqhgKkqSKoSBJqhgKkqSKoSBJqhgKkqSKoSBJqnjnNb2mecaztHsMBe22dn3R+oUutZ/TR5KkiqEgSaoYCpKkivsU9ArO40tjmyMFSVLFUJAkVQwFSVLFUJAkVQwFSVLFUJAkVQwFSVLFUJAkVTx5TWqBJ/VprHCkIEmq1DpSiIhHgReAHcD2zOyNiEnA9cBU4FHgQ5n5TEQE8LfAKcCLwMcz894665PAUYDUrB0jhZMyc0Zm9pb184BVmTkdWFXWAT4ATC8/y4BL21CbJKlJJ6aPFgIryvIKYFFT+9XZsAaYGBGHd6A+SRqz6g6FBP4lItZHxLLSdlhmPglQHg8t7ZOBx5te21faJEltUvfRRydm5hMRcShwW0Q8PETfGKAtd+nUCJdlAEcdddTIVClJAmoeKWTmE+VxM3AzMAt4qn9aqDxuLt37gCObXj4FeGKA91yemb2Z2dvT01Nn+ZI05tQWChGxf0Qc0L8MzAceBG4BlpRuS4CVZfkW4GPRMBt4rn+aSZLUHnVOHx0G3Nw40pS9gX/IzB9GxE+AGyJiKbAJWFz630rjcNSNNA5J/USNtUkjbqhDWz2sVaNFbaGQmb8Ajh+gfSswb4D2BM6qqx5J0vA8o1mSVDEUJEkVQ0GSVDEUJEkVQ0GSVDEUJEkVQ0GSVDEUJEkVQ0GSVDEUJEkVQ0GSVKn7fgqSCu8FrdHAkYIkqWIoSJIqhoIkqWIoSJIqhoIkqWIoSJIqhoIkqWIoSJIqhoIkqWIoSJIqhoIkqVJ7KETEXhFxX0R8r6xPi4h7IuKRiLg+IvYp7fuW9Y3l+al11yZJeqV2jBTOATY0rX8VuDgzpwPPAEtL+1Lgmcx8I3Bx6SdJaqNaQyEipgALgMvLegBzgRtLlxXAorK8sKxTnp9X+kuS2qTukcLXgT8HXi7rBwPPZub2st4HTC7Lk4HHAcrzz5X+kqQ2qe1+ChHxB8DmzFwfEXP6mwfomi081/y+y4BlAEcdddQIVDp2eD1/ScOp8yY7JwJ/GBGnAOOBN9AYOUyMiL3LaGAK8ETp3wccCfRFxN7AgcDTO79pZi4HlgP09vbuEhrSaNZKcBvuqlNt00eZ+YXMnJKZU4EPA3dk5keBO4HTSrclwMqyfEtZpzx/R2b6pS9JbdSJ8xQ+D3w2IjbS2GdwRWm/Aji4tH8WOK8DtUnSmNaWezRn5mpgdVn+BTBrgD7bgMXtqEeSNDDPaJYkVQwFSVKlpVCIiBNbaZMkjW6tjhT+Z4ttkqRRbMgdzRHxLuD3gZ6I+GzTU28A9qqzMElS+w139NE+wOtLvwOa2p/nd+caSJJeI4YMhcz8EfCjiLgqMx9rU02SpA5p9TyFfSNiOTC1+TWZObeOoiRJndFqKPwjcBmNS2DvqK8cSVIntRoK2zPz0lorkSR1XKuHpP5TRHw6Ig6PiEn9P7VWJklqu1ZHCv1XL/1cU1sCvzey5UiSOqmlUMjMaXUXIknqvJZCISI+NlB7Zl49suVIkjqp1emjmU3L44F5wL2AoSB1Ge/Mpj3R6vTRnzSvR8SBwDW1VCRJ6phXe+nsF4HpI1mIJKnzWt2n8E80jjaCxoXw3gzcUFdRkqTOaHWfwtealrcDj2VmXw31SJI6qKXpo3JhvIdpXCn1IOA3dRYlSeqMVu+89iFgLbAY+BBwT0R46WxJeo1pdfrovwEzM3MzQET0ALcDN9ZVmCSp/Vo9+uh1/YFQbN2N10qSRolWv9h/GBH/HBEfj4iPA98Hbh3qBRExPiLWRsS/RsRPI+JLpX1aRNwTEY9ExPURsU9p37esbyzPT331v5Yk6dUYMhQi4o0RcWJmfg74O+A44HjgbmD5MO/9a2BuZh4PzABOjojZwFeBizNzOvAMsLT0Xwo8k5lvBC4u/SRJbTTcSOHrwAsAmfndzPxsZp5LY5Tw9aFemA2/LKvjyk8Cc/ndvogVwKKyvLCsU56fFxGxG7+LJGkPDRcKUzPzgZ0bM3MdjVtzDiki9oqI+4HNwG3AvwPPZub20qUPmFyWJwOPl/ffDjwHHNzC7yBJGiHDhcL4IZ6bMNybZ+aOzJwBTAFm0TgTepdu5XGgUUHu3BARyyJiXUSs27Jly3AlSJJ2w3Ch8JOI+OTOjRGxFFjf6odk5rPAamA2MDEi+g+FnQI8UZb7gCPL++8NHAg8PcB7Lc/M3szs7enpabUESVILhjtP4TPAzRHxUX4XAr3APsCpQ72wnMvw28x8NiImAO+jsfP4TuA04Doad3RbWV5yS1m/uzx/R2buMlKQJNVnyFDIzKeA34+Ik4C3lubvZ+YdLbz34cCKiNiLxojkhsz8XkQ8BFwXERcC9wFXlP5XANdExEYaI4QP7/6vI0naE63eT+FOGn/ht6zsoD5hgPZf0Ni/sHP7NhqX0ZAkdYhnJUuSKq1e+0jSa4i37NRgHClIkiqGgiSpYihIkiqGgiSpYihIkiqGgiSpYihIkiqGgiSpYihIkiqGgiSpYihIkipe++g1ZKjr2XgtG0mtcKQgSaoYCpKkiqEgSaoYCpKkiqEgSaoYCpKkioekShqQt+wcmxwpSJIqhoIkqWIoSJIqtYVCRBwZEXdGxIaI+GlEnFPaJ0XEbRHxSHk8qLRHRFwSERsj4oGIeHtdtUmSBlbnSGE78GeZ+WZgNnBWRBwLnAesyszpwKqyDvABYHr5WQZcWmNtkqQB1BYKmflkZt5bll8ANgCTgYXAitJtBbCoLC8Ers6GNcDEiDi8rvokSbtqyz6FiJgKnADcAxyWmU9CIziAQ0u3ycDjTS/rK207v9eyiFgXEeu2bNlSZ9mSNObUHgoR8XrgJuAzmfn8UF0HaMtdGjKXZ2ZvZvb29PSMVJmSJGoOhYgYRyMQrs3M75bmp/qnhcrj5tLeBxzZ9PIpwBN11idJeqU6jz4K4ApgQ2b+TdNTtwBLyvISYGVT+8fKUUizgef6p5kkSe1R52UuTgTOBP4tIu4vbecDFwE3RMRSYBOwuDx3K3AKsBF4EfhEjbVJkgZQWyhk5v9m4P0EAPMG6J/AWXXVI0kanmc0S5IqhoIkqeKlsyW9akNdXttLa49OjhQkSRVDQZJUMRQkSRVDQZJUMRQkSRVDQZJUMRQkSRVDQZJUMRQkSRVDQZJUMRQkSRWvfSSpNkNdGwm8PlI3cqQgSaoYCpKkitNHo4TDcEnt4EhBklQxFCRJFUNBklQxFCRJFUNBklSpLRQi4sqI2BwRDza1TYqI2yLikfJ4UGmPiLgkIjZGxAMR8fa66pIkDa7OkcJVwMk7tZ0HrMrM6cCqsg7wAWB6+VkGXFpjXZKkQdQWCpn5Y+DpnZoXAivK8gpgUVP71dmwBpgYEYfXVZskaWDtPnntsMx8EiAzn4yIQ0v7ZODxpn59pe3Jnd8gIpbRGE1w1FFH1VutpNp5YmZ36ZYdzTFAWw7UMTOXZ2ZvZvb29PTUXJYkjS3tDoWn+qeFyuPm0t4HHNnUbwrwRJtrk6Qxr92hcAuwpCwvAVY2tX+sHIU0G3iuf5pJktQ+te1TiIjvAHOAQyKiD/gL4CLghohYCmwCFpfutwKnABuBF4FP1FWXJGlwtYVCZg62d2jeAH0TOKuuWiSNbu6Mbp9u2dEsSeoChoIkqWIoSJIqhoIkqWIoSJIqhoIkqWIoSJIqhoIkqdLuq6RK0ojz5LaR40hBklQxFCRJFUNBklRxn0IXcD5UUrdwpCBJqhgKkqSK00eSxgSnaVvjSEGSVDEUJEkVQ0GSVHGfgiQV7ndwpCBJauJIoWb+5SG9tgz1f/q18P/ZkYIkqdJVoRARJ0fEzyJiY0Sc1+l6JGms6ZpQiIi9gG8CHwCOBT4SEcd2tipJGlu6aZ/CLGBjZv4CICKuAxYCD9XxYa3M9Y9EH0ljS7u+W+rafxGZWcsb766IOA04OTP/a1k/E3hnZp69U79lwLKyegzwsxEq4RDgP0bovdphNNU7mmoF663TaKoVRle9u1Prf87MnoGe6KaRQgzQtktiZeZyYPmIf3jEuszsHen3rctoqnc01QrWW6fRVCuMrnpHqtau2acA9AFHNq1PAZ7oUC2SNCZ1Uyj8BJgeEdMiYh/gw8AtHa5JksaUrpk+ysztEXE28M/AXsCVmfnTNpYw4lNSNRtN9Y6mWsF66zSaaoXRVe+I1No1O5olSZ3XTdNHkqQOMxQkSZUxGQoRcWVEbI6IB5vaJkXEbRHxSHk8qJM1Nhuk3gsi4v9GxP3l55RO1tgvIo6MiDsjYkNE/DQizintXbd9h6i1W7ft+IhYGxH/Wur9UmmfFhH3lG17fTlQo+OGqPeqiPg/Tdt3Rqdr7RcRe0XEfRHxvbLeldu23wD17vG2HZOhAFwFnLxT23nAqsycDqwq693iKnatF+DizJxRfm5tc02D2Q78WWa+GZgNnFUuV9KN23ewWqE7t+2vgbmZeTwwAzg5ImYDX6VR73TgGWBpB2tsNli9AJ9r2r73d67EXZwDbGha79Zt22/nemEPt+2YDIXM/DHw9E7NC4EVZXkFsKitRQ1hkHq7UmY+mZn3luUXaPyDnUwXbt8hau1K2fDLsjqu/CQwF7ixtHfFtoUh6+1KETEFWABcXtaDLt22sGu9I2VMhsIgDsvMJ6HxZQEc2uF6WnF2RDxQppc6Ph2zs4iYCpwA3EOXb9+daoUu3bZluuB+YDNwG/DvwLOZub106aOLgm3nejOzf/v+Vdm+F0fEvh0ssdnXgT8HXi7rB9PF25Zd6+23R9vWUBi9LgX+C41h+ZPA/+hsOa8UEa8HbgI+k5nPd7qeoQxQa9du28zckZkzaJzxPwt480Dd2lvV4HauNyLeCnwBeBMwE5gEfL6DJQIQEX8AbM7M9c3NA3Ttim07SL0wAtvWUPidpyLicIDyuLnD9QwpM58q/+FeBr5F4wuiK0TEOBpfstdm5ndLc1du34Fq7eZt2y8znwVW09gXMjEi+k9E7crLwzTVe3KZtsvM/DXw93TH9j0R+MOIeBS4jsa00dfp3m27S70R8e2R2LaGwu/cAiwpy0uAlR2sZVj9X7DFqcCDg/VtpzIPewWwITP/pumprtu+g9Xaxdu2JyImluUJwPto7Ae5EzitdOuKbQuD1vtw0x8HQWOOvuPbNzO/kJlTMnMqjUvs3JGZH6VLt+0g9f7RSGzbrrnMRTtFxHeAOcAhEdEH/AVwEXBDRCwFNgGLO1fhKw1S75xyuFkCjwKf6liBr3QicCbwb2UuGeB8unP7DlbrR7p02x4OrIjGDaleB9yQmd+LiIeA6yLiQuA+GkHXDQar946I6KExPXM/8MedLHIYn6c7t+1grt3TbetlLiRJFaePJEkVQ0GSVDEUJEkVQ0GSVDEUJEkVQ0GSVDEUpA4ox+5LXcdQkIYREV/uv9dCWf+riPjTiPhcRPykXHzsS03P/6+IWF/uIbCsqf2XEfGXEXEP8K6IuCgiHiqv/1qbfy1pQJ68Jg2jXEH1u5n59oh4HfAIjTOf59E42zloXMbjrzPzxxExKTOfLpd2+Anw3szcGhEJnJ6ZN0TEJOBu4E2ZmRExsVwfSOqoMXmZC2l3ZOajEbE1Ik4ADqNxuYOZwPyyDPB6YDrwY+BPI+LU0n5kad8K7KBx8T2A54FtwOUR8X3ge+34XaThGApSay4HPg78J+BKGqOEr2Tm3zV3iog5NC789q7MfDEiVgPjy9PbMnMHQGZuj4hZ5X0+DJxN48qcUkcZClJrbgb+ksbdw86gcSvPL0fEtZn5y4iYDPwWOBB4pgTCm2hc2noX5R4O+2XmrRGxBtjYlt9CGoahILUgM38TEXfSuBPXDuBfIuLNwN2NqxTzS+CPgB8CfxwRDwA/A9YM8pYHACsjYjyNfRLn1v07SK1wR7PUgrKD+V5gcWY+0ul6pLp4SKo0jIg4lsb0zioDQa91jhQkSRVHCpKkiqEgSaoYCpKkiqEgSaoYCpKkyv8HArWsMjkbXSQAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "hist = thinkstats2.Hist(ages, label='agepreg')\n",
    "thinkplot.Hist(hist)\n",
    "thinkplot.Config(xlabel='years', ylabel='Count')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "As an exercise, plot the histogram of pregnancy lengths (column `prglngth`)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAYsAAAEGCAYAAACUzrmNAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjMsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+AADFEAAAaZ0lEQVR4nO3de5RV5Z3m8e8jIBDAG5SOAm1hxBbiirfyEmFlGbWRxAvG0ZZEWzS0RKNRuzvpjokzxqhL0+OKSUzUZsQFZiFGo8ZbEmS8dMIMXrh4Q3SgDWItHSGgBqIo0L/5Y7+Fx+JU7Sqsfc6pc57PWrVq73e/e593F4fznHdf3q2IwMzMrDM7VLsBZmZW+xwWZmaWy2FhZma5HBZmZpbLYWFmZrn6VrsBRRg2bFg0NzdXuxlmZr3KokWL/hQRTeWW1WVYNDc3s3Dhwmo3w8ysV5H0WkfLfBjKzMxyOSzMzCyXw8LMzHLV5TmLcjZt2kRraysbN26sdlN6lQEDBjBixAj69etX7aaYWRU1TFi0trYyZMgQmpubkVTt5vQKEcHatWtpbW1l1KhR1W6OmVVRwxyG2rhxI0OHDnVQdIMkhg4d6t6YmTVOWAAOiu3gv5mZQYOFhZmZbZ+GOWfR3kVXz+nR7f3s8q/06PbKeeKJJ7j++ut56KGHtmv9mTNnMmHCBPbaay/go5sXhw0b1pPNNLM61LBhUau2bNlCnz59Ctn2zJkzOeCAA7aGhVlvVe7LXiW+sDUyh0UFrVy5kokTJ3LEEUewZMkS9ttvP26//XbGjh3L1772NR555BEuuugiPv3pTzN16lQGDRrE+PHj+e1vf8uLL774sW19//vfZ9WqVbz66qusWrWKSy+9lIsvvhiAq666itmzZzNy5EiGDRvGoYceurUXceaZZzJw4EAWLFgAwI033siDDz7Ipk2buPvuu9l///0r/ncxs9rncxYV9sorrzBt2jSef/55dtppJ2666SYgu59h/vz5TJ48mXPPPZdbbrmFBQsWdNrLePnll5k7dy5PP/00V155JZs2bWLhwoXcc889LFmyhHvvvXfrGFmnnXYaLS0tzJ49m2effZaBAwcCMGzYMBYvXswFF1zA9ddfX/wfwMx6JYdFhY0cOZJx48YBcNZZZzF//nwAzjjjDADeeecd1q9fz1FHHQXAV7/61Q63dcIJJ9C/f3+GDRvG7rvvzltvvcX8+fOZNGkSAwcOZMiQIZx00kmdtufUU08F4NBDD2XlypWfdPfMrE45LCqs/aWobfODBg0Cshvhuqp///5bp/v06cPmzZu7tX7pNtrWNzMrx2FRYatWrdp6vmDOnDmMHz/+Y8t33XVXhgwZwpNPPgnAnXfe2a3tjx8/ngcffJCNGzeyYcMGHn744a3LhgwZwvr16z/hHphZI2rYE9zVunJizJgxzJo1i69//euMHj2aCy64gBtvvPFjdWbMmMF5553HoEGDOProo9l55527vP3DDjuMk08+mQMPPJC9996blpaWreufc845nH/++R87wW1m1hXq7mGL3qClpSXaP/xo2bJljBkzpkotyqxcuZITTzxxmyub2tuwYQODBw8G4LrrruPNN9/kJz/5SZdfp2399957j89//vNMnz6dQw45ZLvbXQt/O7NSvnS2GJIWRURLuWUN27OoZQ8//DDXXnstmzdvZu+992bmzJndWn/atGm89NJLbNy4kSlTpnyioDAzA4dFRTU3N+f2KiC7Mqrt6qjtcccdd2z3umZm5TTUCe56PORWNP/NzAwaKCwGDBjA2rVr/eHXDW3PsxgwYEC1m2JmVdYwh6FGjBhBa2sra9asqXZTepW2J+WZWWNrmLDo16+fn/ZmZradGuYwlJmZbT+HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZrsLDQlIfSUskPZTmR0l6StJySb+UtGMq75/mV6TlzSXbuCyVvyLp+KLbbGZmH1eJnsUlwLKS+R8CN0TEaOBtYGoqnwq8HRH7AjekekgaC0wGPgNMBG6S1KcC7TYzs6TQsJA0AjgBuDXNCzgG+FWqMgs4JU1PSvOk5cem+pOAOyPig4j4I7ACOLzIdpuZ2ccV3bP4MfDPwH+m+aHAOxGxOc23AsPT9HDgdYC0/N1Uf2t5mXW2kjRN0kJJC/2cbTOznlVYWEg6EVgdEYtKi8tUjZxlna3zUUHE9IhoiYiWpqambrfXzMw61rfAbY8DTpb0JWAAsBNZT2MXSX1T72EE8Eaq3wqMBFol9QV2BtaVlLcpXcfMzCqgsJ5FRFwWESMiopnsBPVjEXEm8DhwWqo2Bbg/TT+Q5knLH4uISOWT09VSo4DRwNNFtdvMzLZVZM+iI/8C3CnpamAJMCOVzwB+IWkFWY9iMkBELJV0F/ASsBm4MCK2VL7ZZmaNqyJhERFPAE+k6VcpczVTRGwETu9g/WuAa4proZmZdcZ3cJuZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVmuwsJC0gBJT0t6TtJSSVem8lGSnpK0XNIvJe2Yyvun+RVpeXPJti5L5a9IOr6oNpuZWXlF9iw+AI6JiAOBg4CJko4EfgjcEBGjgbeBqan+VODtiNgXuCHVQ9JYYDLwGWAicJOkPgW228zM2iksLCKzIc32Sz8BHAP8KpXPAk5J05PSPGn5sZKUyu+MiA8i4o/ACuDwotptZmbbKvSchaQ+kp4FVgPzgP8A3omIzalKKzA8TQ8HXgdIy98FhpaWl1mn9LWmSVooaeGaNWuK2B0zs4ZVaFhExJaIOAgYQdYbGFOuWvqtDpZ1VN7+taZHREtEtDQ1NW1vk83MrIyKXA0VEe8ATwBHArtI6psWjQDeSNOtwEiAtHxnYF1peZl1zMysAoq8GqpJ0i5peiBwHLAMeBw4LVWbAtyfph9I86Tlj0VEpPLJ6WqpUcBo4Omi2m1mZtvqm19lu+0JzEpXLu0A3BURD0l6CbhT0tXAEmBGqj8D+IWkFWQ9iskAEbFU0l3AS8Bm4MKI2FJgu83MrJ3CwiIingcOLlP+KmWuZoqIjcDpHWzrGuCanm6jmZl1je/gNjOzXA4LMzPL5bAwM7NcDgszM8vVpbCQNK4rZWZmVp+62rO4sYtlZmZWhzq9dFbS54CjgCZJ/1iyaCfAI7+amTWIvPssdgQGp3pDSsr/zEd3YZuZWZ3rNCwi4t+Bf5c0MyJeq1CbzMysxnT1Du7+kqYDzaXrRMQxRTTKzMxqS1fD4m7gFuBWwOMymZk1mK6GxeaIuLnQlpiZWc3q6qWzD0r6hqQ9Je3W9lNoy8zMrGZ0tWfR9pyJb5eUBbBPzzbHzMxqUZfCIiJGFd0QMzOrXV0KC0lnlyuPiNt7tjlmZlaLunoY6rCS6QHAscBiwGFhZtYAunoY6pul85J2Bn5RSIvMzKzmbO8Q5e8Bo3uyIWZmVru6es7iQbKrnyAbQHAMcFdRjTIzs9rS1XMW15dMbwZei4jWAtpjZmY1qEuHodKAgi+TjTy7K/BhkY0yM7Pa0tUn5f0t8DRwOvC3wFOSPES5mVmD6OphqO8Bh0XEagBJTcD/An5VVMPMzKx2dPVqqB3agiJZ2411zcysl+tqz+J3kuYCc9L8GcBvimmSmZnVmrxncO8L7BER35Z0KjAeELAAmF2B9pmZWQ3IO5T0Y2A9QETcGxH/GBH/QNar+HHRjTMzs9qQFxbNEfF8+8KIWEj2iFUzM2sAeWExoJNlA3uyIWZmVrvywuIZSee1L5Q0FVhUTJPMzKzW5F0NdSlwn6Qz+SgcWoAdgS8X2TAzM6sdnYZFRLwFHCXpC8ABqfjhiHis8JaZmVnN6OrzLB4HHi+4LWZmVqMKuwtb0khJj0taJmmppEtS+W6S5klann7vmsol6aeSVkh6XtIhJduakuovlzSlqDabmVl5RQ7ZsRn4p4gYAxwJXChpLPAd4NGIGA08muYBvkj2QKXRwDTgZsjCBbgCOAI4HLiiLWDMzKwyCguLiHgzIhan6fXAMmA4MAmYlarNAk5J05OA2yPzJLCLpD2B44F5EbEuIt4G5gETi2q3mZltqyKDAUpqBg4GniIbPuRNyAIF2D1VGw68XrJaayrrqLz9a0yTtFDSwjVr1vT0LpiZNbTCw0LSYOAe4NKI+HNnVcuURSflHy+ImB4RLRHR0tTUtH2NNTOzsgoNC0n9yIJidkTcm4rfSoeXSL/bhj5vBUaWrD4CeKOTcjMzq5Air4YSMANYFhE/Kln0ANB2RdMU4P6S8rPTVVFHAu+mw1RzgQmSdk0ntiekMjMzq5CuPs9ie4wD/g54QdKzqey7wHXAXWnIkFVkj2qFbCTbLwErgPeAcwEiYp2kq4BnUr0fRMS6AtttZmbtFBYWETGf8ucbAI4tUz+ACzvY1m3AbT3XOjMz6w4/GtXMzHI5LMzMLJfDwszMcjkszMwsl8PCzMxyOSzMzCyXw8LMzHI5LMzMLJfDwszMcjkszMwsl8PCzMxyOSzMzCyXw8LMzHI5LMzMLJfDwszMcjkszMwsl8PCzMxyOSzMzCyXw8LMzHI5LMzMLJfDwszMcjkszMwsl8PCzMxyOSzMzCyXw8LMzHI5LMzMLJfDwszMcjkszMwsl8PCzMxyOSzMzCyXw8LMzHI5LMzMLJfDwszMcvWtdgPMzHrKRVfP2absZ5d/pQotqT/uWZiZWa7CwkLSbZJWS3qxpGw3SfMkLU+/d03lkvRTSSskPS/pkJJ1pqT6yyVNKaq9ZmbWsSJ7FjOBie3KvgM8GhGjgUfTPMAXgdHpZxpwM2ThAlwBHAEcDlzRFjBmZlY5hYVFRPweWNeueBIwK03PAk4pKb89Mk8Cu0jaEzgemBcR6yLibWAe2waQmZkVrNLnLPaIiDcB0u/dU/lw4PWSeq2prKPybUiaJmmhpIVr1qzp8YabmTWyWjnBrTJl0Un5toUR0yOiJSJampqaerRxZmaNrtJh8VY6vET6vTqVtwIjS+qNAN7opNzMzCqo0mHxANB2RdMU4P6S8rPTVVFHAu+mw1RzgQmSdk0ntiekMjMzq6DCbsqTNAc4GhgmqZXsqqbrgLskTQVWAaen6r8BvgSsAN4DzgWIiHWSrgKeSfV+EBHtT5qbmVnBCguLiOjotsljy9QN4MIOtnMbcFsPNs3MzLqpVk5wm5lZDXNYmJlZLoeFmZnlcliYmVkuh4WZmeVyWJiZWS6HhZmZ5XJYmJlZLoeFmZnlcliYmVkuh4WZmeUqbGwoM7NP6qKr52xT9rPLOxp2zorksGgQ5f7Tgf/jWWNw6HxyPgxlZma53LOoce4RmFktcM/CzMxyOSzMzCyXw8LMzHL5nIX1CJ9bsU/CVyvVPvcszMwsl3sWFVb0N/COtm9m9km4Z2FmZrncszCzHudzEPXHYWFmFVNrIVJr7allPgxlZma5HBZmZpbLYWFmZrl8zsJ6Bd/0Z1Zd7lmYmVku9yysLH+Trw++2sd6isOiRnT3zutq3antO8TNGpPDoiD+UK0P/mbemPzvvi2HRYNzqFkpf0haRxwWZlXkD2frLRwWPcDfzs2s3vWasJA0EfgJ0Ae4NSKuq3KTzBqee0aNo1eEhaQ+wM+BvwFagWckPRARL1W3Zba9iu6NdffS35760Cv6w7NaH84OhUwj/x16RVgAhwMrIuJVAEl3ApOAQsKiozdEI79Rtle1QqGn6ndnO529F3p7fetcb/my8UkoIqrdhlySTgMmRsTfp/m/A46IiItK6kwDpqXZvwZe6YGXHgb8qQe201t4f+tbI+1vI+0r9Nz+7h0RTeUW9JaehcqUfSzlImI6ML1HX1RaGBEtPbnNWub9rW+NtL+NtK9Qmf3tLWNDtQIjS+ZHAG9UqS1mZg2nt4TFM8BoSaMk7QhMBh6ocpvMzBpGrzgMFRGbJV0EzCW7dPa2iFhagZfu0cNavYD3t7410v420r5CBfa3V5zgNjOz6uoth6HMzKyKHBZmZpbLYdEBSRMlvSJphaTvVLs9PU3SbZJWS3qxpGw3SfMkLU+/d61mG3uKpJGSHpe0TNJSSZek8nrd3wGSnpb0XNrfK1P5KElPpf39ZbpYpG5I6iNpiaSH0nzd7q+klZJekPSspIWprND3s8OijJLhRb4IjAW+ImlsdVvV42YCE9uVfQd4NCJGA4+m+XqwGfiniBgDHAlcmP4963V/PwCOiYgDgYOAiZKOBH4I3JD2921gahXbWIRLgGUl8/W+v1+IiINK7q8o9P3ssChv6/AiEfEh0Da8SN2IiN8D69oVTwJmpelZwCkVbVRBIuLNiFicpteTfaAMp373NyJiQ5rtl34COAb4VSqvm/0FkDQCOAG4Nc2LOt7fDhT6fnZYlDcceL1kvjWV1bs9IuJNyD5ggd2r3J4eJ6kZOBh4ijre33RI5llgNTAP+A/gnYjYnKrU23v6x8A/A/+Z5odS3/sbwCOSFqWhjqDg93OvuM+iCnKHF7HeR9Jg4B7g0oj4c/blsz5FxBbgIEm7APcBY8pVq2yriiHpRGB1RCySdHRbcZmqdbG/ybiIeEPS7sA8SS8X/YLuWZTXqMOLvCVpT4D0e3WV29NjJPUjC4rZEXFvKq7b/W0TEe8AT5Cdq9lFUtsXxHp6T48DTpa0kuyQ8TFkPY163V8i4o30ezXZl4HDKfj97LAor1GHF3kAmJKmpwD3V7EtPSYdv54BLIuIH5Usqtf9bUo9CiQNBI4jO0/zOHBaqlY3+xsRl0XEiIhoJvu/+lhEnEmd7q+kQZKGtE0DE4AXKfj97Du4OyDpS2TfTtqGF7mmyk3qUZLmAEeTDW38FnAF8GvgLuCvgFXA6RHR/iR4ryNpPPAH4AU+Oqb9XbLzFvW4v58lO8HZh+wL4V0R8QNJ+5B9894NWAKcFREfVK+lPS8dhvpWRJxYr/ub9uu+NNsXuCMirpE0lALfzw4LMzPL5cNQZmaWy2FhZma5HBZmZpbLYWFmZrkcFmZmlsthYRUjaUsaJfNFSXdL+lS129STJG3Ir/WJtn+OpL1K5ldKGtaF9Q6WdGtBbWouHbk4p+6Okn5fcqOc9SIOC6uk99MomQcAHwLnly5Uxu/Jjp0D7JVXqYzvAjf2bFO6Lw3K+ShwRrXbYt3n/5hWLX8A9k3fTJdJuglYDIyUNEHSAkmLUw9kMGQ3Skp6WdJ8ST8teW7B95U9n+MJSa9KurjtRST9Og22trRkwDUkbZB0TXrmw5OS9kjle0i6L5U/J+koSVcpPQMj1bmm9DU6k+6mvkfSM+lnXBfa/N/Sfs6TNEfStySdBrQAs1PvbGCq/s30d3pB0v5lXn8I8NmIeC7NvyBplxTMayWdncp/Iek4ZQMQ/o/U1uclfb1kW98uKb+yzGvto+x5EodJ+oyyZ2o8m+qPTtV+DZzZlb+d1ZiI8I9/KvIDbEi/+5INRXAB0Ex2V/WRadkw4PfAoDT/L8B/BwaQjQQ8KpXPAR5K098H/g/QP62/FuiXlu2Wfg8kGxJhaJoP4KQ0/a/A5Wn6l2QDDUJ2B/TOqY2LU9kOZCO4Du1o/9qV3QGMT9N/RTbkSIdtJguEZ1N7hwDLye5IhmyMp5aSba8EvpmmvwHcWub1vwDcUzJ/C9lQ3geQDWvzP1P5cmAwMK3kb9EfWAiMIhtSYjrZAH07AA8Bn09/mxeBvya7S/qgtO6NwJlpekdgYMnfdE2134v+6f6Pjx1aJQ1UNmw2ZD2LGWSHVV6LiCdT+ZFkD5z639mQTuwILAD2B16NiD+menPIPtjaPBzZUA4fSFoN7EE2IOTFkr6c6owERpN9MH9I9oEHsAj4mzR9DHA2bB259V3g3fQt/OC03SURsbaL+3wcMFYfjXC7U9u4Ph20eTxwf0S8DyDpwZzttw2KuAg4tczyPYE1JfN/IPuQfw24GZgmaTiwLiI2SJoAfDb1ZCALy9FkYTGBLBAgC5bRZMNKNJGF/3+NiKVp+QLge8qeM3FvRCyH7G8q6UNJQyJ7toj1Eg4Lq6T3I+Kg0oL0IfqX0iJgXkR8pV29g3O2XTrmzxagr7Jxgo4DPhcR70l6gqyHArAp0lfdtvo527+V7JzBfwFuy6lbaof0+u+XFqb93qbNlB9auzNt2+hoH97no32GrNd2IVkv53vAl8kG2/tDW9PIeitz27X3eODaiPi3duXNZIH6Otnor0sBIuIOSU+R9WLmSvr7iHgsrdYf2NjN/bQq8zkLqzVPAuMk7Qsg6VOS9gNeBvZJH07QtZOkOwNvp6DYn6zXkudRssNjbQ8Q2imV30f2GNrDgLkdrFvOI8BFbTOSDuqkLsB84CRlz9EeTPZh22Y92aGp7lgG7Ns2ExGvkx32Gh0Rr6bX+xYfhcVc4AJlQ7ojaT9lI5vOBb5Wcv5ouLJnKUDWSzsFOFvSV9Pyfch6gj8lGw31s6l8KNlhqE3d3A+rMvcsrKZExBpJ5wBzJPVPxZdHxP+V9A3gd5L+BDzdhc39Djhf0vPAK2RBlOcSYLqkqWTf1i8AFkTEh5IeJ3v62pYO1v2UpNaS+R8BFwM/T23oS/bN/vxyKwNExDOSHgCeIztUtJDsmztkz02/RdL7wOe6sC9ExMuSdm532OcpsnMHkIXEtWShAVkPqhlYrKz7swY4JSIekTQGWJB6RRuAs8j+RkTEX5Q9hGiepL+QHUo8S9Im4P8BP0jb/wLwm6603WqLR521XkPS4HRcXcDPgeURcUOFXnsHsqu1Tm87/l7ga7Xt56fIwmVapGeIb+f2/gFYHxGF3GvRzbbcC1wWEa9Uuy3WPT4MZb3JeekE+VKyQ0z/llO/R0gaC6wAHi06KJLpaT8Xk13JtN1BkdzMx8+PVIWyB4n92kHRO7lnYWZmudyzMDOzXA4LMzPL5bAwM7NcDgszM8vlsDAzs1z/H5Y9X8jfpo4KAAAAAElFTkSuQmCC\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "prglngth = np.floor(preg.prglngth)\n",
    "histprglngth = thinkstats2.Hist(prglngth, label='prglngth')\n",
    "thinkplot.Hist(histprglngth)\n",
    "thinkplot.Config(xlabel='Pregnancy Length (weeks)', ylabel='Count')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "`Hist` provides smallest, which select the lowest values and their frequencies."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "10.0 2\n",
      "11.0 1\n",
      "12.0 1\n",
      "13.0 14\n",
      "14.0 43\n",
      "15.0 128\n",
      "16.0 242\n",
      "17.0 398\n",
      "18.0 546\n",
      "19.0 559\n"
     ]
    }
   ],
   "source": [
    "for weeks, freq in hist.Smallest(10):\n",
    "    print(weeks, freq)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Use `Largest` to display the longest pregnancy lengths."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "44.0 1\n",
      "43.0 1\n",
      "42.0 2\n",
      "41.0 14\n",
      "40.0 21\n",
      "39.0 34\n",
      "38.0 55\n",
      "37.0 83\n",
      "36.0 99\n",
      "35.0 138\n"
     ]
    }
   ],
   "source": [
    "for weeks, freq in hist.Largest(10):\n",
    "    print(weeks, freq)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "From live births, we can select first babies and others using `birthord`, then compute histograms of pregnancy length for the two groups."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [],
   "source": [
    "firsts = live[live.birthord == 1]\n",
    "others = live[live.birthord != 1]\n",
    "\n",
    "first_hist = thinkstats2.Hist(firsts.prglngth, label='first')\n",
    "other_hist = thinkstats2.Hist(others.prglngth, label='other')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can use `width` and `align` to plot two histograms side-by-side."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "width = 0.45\n",
    "thinkplot.PrePlot(2)\n",
    "thinkplot.Hist(first_hist, align='right', width=width)\n",
    "thinkplot.Hist(other_hist, align='left', width=width)\n",
    "thinkplot.Config(xlabel='weeks', ylabel='Count', xlim=[27, 46])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "`Series` provides methods to compute summary statistics:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [],
   "source": [
    "mean = live.prglngth.mean()\n",
    "var = live.prglngth.var()\n",
    "std = live.prglngth.std()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Here are the mean and standard deviation:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(38.56055968517709, 2.702343810070593)"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "mean, std"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "As an exercise, confirm that `std` is the square root of `var`:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(7.302662067826851, 7.302662067826851)"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "var, std**2"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Here's are the mean pregnancy lengths for first babies and others:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(38.60095173351461, 38.52291446673706)"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "firsts.prglngth.mean(), others.prglngth.mean()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "And here's the difference (in weeks):"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.07803726677754952"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "firsts.prglngth.mean() - others.prglngth.mean()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This functon computes the Cohen effect size, which is the difference in means expressed in number of standard deviations:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [],
   "source": [
    "def CohenEffectSize(group1, group2):\n",
    "    \"\"\"Computes Cohen's effect size for two groups.\n",
    "    \n",
    "    group1: Series or DataFrame\n",
    "    group2: Series or DataFrame\n",
    "    \n",
    "    returns: float if the arguments are Series;\n",
    "             Series if the arguments are DataFrames\n",
    "    \"\"\"\n",
    "    diff = group1.mean() - group2.mean()\n",
    "\n",
    "    var1 = group1.var()\n",
    "    var2 = group2.var()\n",
    "    n1, n2 = len(group1), len(group2)\n",
    "\n",
    "    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)\n",
    "    d = diff / np.sqrt(pooled_var)\n",
    "    return d"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Compute the Cohen effect size for the difference in pregnancy length for first babies and others."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.028879044654449883"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "CohenEffectSize(firsts.prglngth, others.prglngth)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "## Exercises"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Using the variable `totalwgt_lb`, investigate whether first babies are lighter or heavier than others. \n",
    "\n",
    "Compute Cohen’s effect size to quantify the difference between the groups.  How does it compare to the difference in pregnancy length?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "7.201094430437772\n",
      "7.325855614973262\n"
     ]
    }
   ],
   "source": [
    "print(firsts.totalwgt_lb.mean())\n",
    "print(others.totalwgt_lb.mean())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "-0.088672927072602"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Based on the Cohen Effect of the difference between weights of first babies vs. others, we see that the difference in standard deviations is about 0.089. The fact that the Cohen's Effect is negative tells us that the first babies are relatively lighter in weight than others."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "For the next few exercises, we'll load the respondent file:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [],
   "source": [
    "resp = nsfg.ReadFemResp()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Make a histogram of <tt>totincr</tt> the total income for the respondent's family.  To interpret the codes see the [codebook](http://www.icpsr.umich.edu/nsfg6/Controller?displayPage=labelDetails&fileCode=FEM&section=R&subSec=7876&srtLabel=607543)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "histtotinc = thinkstats2.Hist(resp.totincr, label='totinc')\n",
    "thinkplot.Hist(histtotinc)\n",
    "thinkplot.Config(xlabel='Total Income', ylabel='Count')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The most common total family income bracket is 14 which corresponds to $75,000 or more."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Make a histogram of <tt>age_r</tt>, the respondent's age at the time of interview."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAYUAAAEKCAYAAAD9xUlFAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjMsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+AADFEAAAY7ElEQVR4nO3de5SddX3v8fdHiAmXlGv0QBJMWlMEEQNEjMaqBQ8gericBQrLC6Uec7TYBqpURU+LVZZaOYgUD5QWFCkitEDFu4BIF6tcJDGCEFSqXKbhaKQKeBQ08D1/7GceN8kksyGzZ2dm3q+1Zs2zf89lvj+eMJ/5Pc+zfztVhSRJAM8YdAGSpM2HoSBJahkKkqSWoSBJahkKkqSWoSBJavUtFJLMSHJLku8kuSPJB5r2+UluTvKDJJcmeWbTPr15fXezfl6/apMkjayfI4XHgAOq6oXAQuCQJIuBjwIfr6oFwM+AtzTbvwX4WVU9F/h4s50kaRz1LRSq4xfNy2nNVwEHAP/ctF8IHNEsH968pll/YJL0qz5J0vq27OfBk2wBLAeeC3wS+Hfg51W1ttlkCJjdLM8G7geoqrVJHgJ2An66zjGXAksBttlmm/2e97zn9bMLkjTpLF++/KdVNWukdX0Nhap6HFiYZHvgSmCPkTZrvo80KlhvDo6qOg84D2DRokV16623jlG1kjQ1JLl3Q+vG5emjqvo58E1gMbB9kuEwmgOsbpaHgLkAzfrtgP8cj/okSR39fPpoVjNCIMlWwKuAVcB1wFHNZscBn2+Wr2pe06z/RjlbnySNq35ePtoFuLC5r/AM4LKq+mKSO4HPJfkQ8G3g/Gb784GLktxNZ4RwTB9rkySNoG+hUFW3AfuM0P5DYP8R2h8Fju5XPZKmpt/85jcMDQ3x6KOPDrqUcTdjxgzmzJnDtGnTet6nrzeaJWnQhoaGmDlzJvPmzWMqPeVeVTz44IMMDQ0xf/78nvdzmgtJk9qjjz7KTjvtNKUCASAJO+2001MeIRkKkia9qRYIw55Ovw0FSVLLewqSppR3fOiSMT3e2e8/dkyPN2iOFCRpkli7du3oG43CkYIkjYMjjjiC+++/n0cffZRly5axdOlSzj//fD760Y+y6667smDBAqZPn87ZZ5/NmjVreNvb3sZ9990HwJlnnsmSJUtGPO6pp57K6tWrueeee9h555357Gc/u0l1GgqSNA4uuOACdtxxR371q1/xohe9iNe85jV88IMfZMWKFcycOZMDDjiAF77whQAsW7aMk046iZe97GXcd999HHzwwaxatWqDx16+fDk33HADW2211SbXaShI0jg466yzuPLKKwG4//77ueiii3jFK17BjjvuCMDRRx/N97//fQCuueYa7rzzznbfhx9+mEceeYSZM2eOeOzDDjtsTAIBDAVJ6rtvfvObXHPNNdx4441svfXWvPKVr2T33Xff4F//TzzxBDfeeGPPv+i32WabMavVUJD6pJenXCbbkysa2UMPPcQOO+zA1ltvzV133cVNN93EW9/6Vq6//np+9rOfMXPmTC6//HJe8IIXAHDQQQdx9tlnc/LJJwOwcuVKFi5cOC61GgqSppRBBPEhhxzCueeey957783uu+/O4sWLmT17NqeccgovfvGL2XXXXdlzzz3ZbrvtgM6lphNOOIG9996btWvX8vKXv5xzzz13XGo1FCSpz6ZPn85XvvKV9doXLVrE0qVLWbt2LUceeSQHHXQQADvvvDOXXnppT8c+9dRTx7JU36cgSYNy6qmnsnDhQvbaay/mz5/PEUccMfpOfeZIQZIG5PTTT+9520996lN84hOfeFLbkiVL+OQnPzmmNRkKkjQBHH/88Rx//PF9/zlePpI06U3VT/Z9Ov02FCRNajNmzODBBx+ccsEw/CE7M2bMeEr7eflI0qQ2Z84choaGWLNmzaBLGXfDH8f5VBgKkia1adOmPaWPo5zqDAWNyHfjSlOT9xQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS3f0axN4jufpcmlbyOFJHOTXJdkVZI7kixr2hcmuSnJyiS3Jtm/aU+Ss5LcneS2JPv2qzZJ0sj6OVJYC7yzqlYkmQksT3I18DfAB6rqK0kObV6/Eng1sKD5ejFwTvNdkjRO+jZSqKoHqmpFs/wIsAqYDRTwO81m2wGrm+XDgc9Ux03A9kl26Vd9kqT1jcs9hSTzgH2Am4ETga8lOZ1OKL202Ww2cH/XbkNN2wPrHGspsBRgt91262fZkjTl9P3poyTbApcDJ1bVw8DbgZOqai5wEnD+8KYj7L7eRyVV1XlVtaiqFs2aNatfZUvSlNTXUEgyjU4gXFxVVzTNxwHDy/8E7N8sDwFzu3afw28vLUmSxkE/nz4KnVHAqqo6o2vVauAVzfIBwA+a5auANzdPIS0GHqqqJ106kiT1Vz/vKSwB3gTcnmRl03YK8FbgE0m2BB6luT8AfBk4FLgb+CVwfB9rkzYbvtdDm5O+hUJV3cDI9wkA9hth+wJO6Fc9kqTROc2FJKllKEiSWoaCJKllKEiSWoaCJKllKEiSWoaCJKllKEiSWn7ymjRB9PLOZ/Ddz9o0jhQkSS1DQZLUMhQkSS3vKUgad94f2Xw5UpAktQwFSVLLUJAktbynoHHhp4tJE4MjBUlSy1CQJLUMBUlSy3sKU8hEeDbcew/SYDlSkCS1HClIGjOO9CY+RwqSpJahIElqGQqSpJahIElqGQqSpJahIElq+Uiq9BRNxccup2Kfp6q+jRSSzE1yXZJVSe5Isqxr3Z8m+V7T/jdd7e9Ncnez7uB+1SZJGlk/RwprgXdW1YokM4HlSa4Gng0cDuxdVY8leRZAkj2BY4DnA7sC1yT5/ap6vI81SpK69G2kUFUPVNWKZvkRYBUwG3g78JGqeqxZ95Nml8OBz1XVY1X1I+BuYP9+1SdJWt+43FNIMg/YB7gZ+BjwB0lOAx4F3lVV36ITGDd17TbUtK17rKXAUoDddtutr3Vr8zURJveTJqK+P32UZFvgcuDEqnqYThDtACwGTgYuSxIgI+xe6zVUnVdVi6pq0axZs/pYuSRNPX0NhSTT6ATCxVV1RdM8BFxRHbcATwA7N+1zu3afA6zuZ32SpCfr59NHAc4HVlXVGV2r/gU4oNnm94FnAj8FrgKOSTI9yXxgAXBLv+qTJK2vn/cUlgBvAm5PsrJpOwW4ALggyXeBXwPHVVUBdyS5DLiTzpNLJ/jkUW98hlzSWOlbKFTVDYx8nwDgjRvY5zTgtH7VJEnaON/RLE1Cjh71dDn3kSSp5UhB0mZtrEc9jqI2zpGCJKnlSEGTnn8Zbpj/bbQuRwqSpJahIElqGQqSpJb3FCRpHEyUmX0dKUiSWo4UpIZP4kiOFCRJXXoKhSRLemmTJE1svY4U/rbHNknSBLbRewpJXgK8FJiV5M+7Vv0OsEU/C5OkQZqq95hGu9H8TGDbZruZXe0PA0f1qyhJ0mBsNBSq6nrg+iSfrqp7x6kmSdKA9PpI6vQk5wHzuvepqgP6UZQkaTB6DYV/As4F/gHwc5MlaZLqNRTWVtU5fa1EkjRwvT6S+oUkf5JklyQ7Dn/1tTJJ0rjrdaRwXPP95K62An53bMuRJA1ST6FQVfP7XYgkafB6CoUkbx6pvao+M7blSJIGqdfLRy/qWp4BHAisAAwFSZpEer189Kfdr5NsB1zUl4okSQPzdKfO/iWwYCwLkSQNXq/3FL5A52kj6EyEtwdwWb+KkiQNRq/3FE7vWl4L3FtVQ32oR5I0QD1dPmomxruLzkypOwC/7mdRkqTB6PWT114H3AIcDbwOuDmJU2dL0iTT643m9wEvqqrjqurNwP7A/9rYDknmJrkuyaokdyRZts76dyWpJDs3r5PkrCR3J7ktyb5Pp0OSpKev13sKz6iqn3S9fpDRA2Ut8M6qWpFkJrA8ydVVdWeSucB/Be7r2v7VdJ5oWgC8GDin+T5lTdVPfpI0OL2OFL6a5GtJ/ijJHwFfAr68sR2q6oGqWtEsPwKsAmY3qz8O/AW/faIJ4HDgM9VxE7B9kl1674okaVON9hnNzwWeXVUnJ/nvwMuAADcCF/f6Q5LMA/ahcy/iMOA/quo7Sbo3mw3c3/V6qGl7YJ1jLQWWAuy22269liBJ6sFoI4UzgUcAquqKqvrzqjqJzijhzF5+QJJtgcuBE+lcUnof8JcjbTpCW63XUHVeVS2qqkWzZs3qpQRJUo9GC4V5VXXbuo1VdSudj+bcqCTT6ATCxVV1BfB7wHzgO0nuAeYAK5L8Fzojg7ldu88BVvfQB0nSGBktFGZsZN1WG9sxnWtD5wOrquoMgKq6vaqeVVXzqmoenSDYt6r+L3AV8ObmKaTFwENV9cCGji9JGnujhcK3krx13cYkbwGWj7LvEuBNwAFJVjZfh25k+y8DPwTuBv4e+JNRji9JGmOjPZJ6InBlkjfw2xBYBDwTOHJjO1bVDYx8n6B7m3ldywWcMEo9kqQ+2mgoVNWPgZcm+UNgr6b5S1X1jb5XJkkad71+nsJ1wHV9rkWSNGBP9/MUJEmTkKEgSWr1OveRxpBzGknamEH+jnCkIElqOVIYQ44AJE10jhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSS1DQZLUMhQkSa0tB13ARPCOD10y6jZnv//YcahEkvrLkYIkqdW3UEgyN8l1SVYluSPJsqb9Y0nuSnJbkiuTbN+1z3uT3J3ke0kO7ldtkqSR9XOksBZ4Z1XtASwGTkiyJ3A1sFdV7Q18H3gvQLPuGOD5wCHA/0myRR/rkySto2+hUFUPVNWKZvkRYBUwu6q+XlVrm81uAuY0y4cDn6uqx6rqR8DdwP79qk+StL5xuaeQZB6wD3DzOqv+GPhKszwbuL9r3VDTtu6xlia5Ncmta9asGftiJWkK63soJNkWuBw4saoe7mp/H51LTBcPN42we63XUHVeVS2qqkWzZs3qR8mSNGX19ZHUJNPoBMLFVXVFV/txwGuBA6tq+Bf/EDC3a/c5wOp+1idJerJ+Pn0U4HxgVVWd0dV+CPBu4LCq+mXXLlcBxySZnmQ+sAC4pV/1SZLW18+RwhLgTcDtSVY2bacAZwHTgas7ucFNVfW2qrojyWXAnXQuK51QVY/3sT5J0jr6FgpVdQMj3yf48kb2OQ04rV81SZI2bspOc9HL1BXg9BWSphanuZAktQwFSVLLUJAktabsPQVJk8ugprifbFPrO1KQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSy1CQJLUMBUlSq2+hkGRukuuSrEpyR5JlTfuOSa5O8oPm+w5Ne5KcleTuJLcl2bdftUmSRtbPkcJa4J1VtQewGDghyZ7Ae4Brq2oBcG3zGuDVwILmaylwTh9rkySNoG+hUFUPVNWKZvkRYBUwGzgcuLDZ7ELgiGb5cOAz1XETsH2SXfpVnyRpfeNyTyHJPGAf4Gbg2VX1AHSCA3hWs9ls4P6u3YaatnWPtTTJrUluXbNmTT/LlqQpp++hkGRb4HLgxKp6eGObjtBW6zVUnVdVi6pq0axZs8aqTEkSfQ6FJNPoBMLFVXVF0/zj4ctCzfefNO1DwNyu3ecAq/tZnyTpyfr59FGA84FVVXVG16qrgOOa5eOAz3e1v7l5Cmkx8NDwZSZJ0vjYso/HXgK8Cbg9ycqm7RTgI8BlSd4C3Acc3az7MnAocDfwS+D4PtYmSRpB30Khqm5g5PsEAAeOsH0BJ/SrHknS6HxHsySpZShIklqGgiSpZShIklqGgiSpZShIklqGgiSpZShIklqGgiSpZShIklqGgiSpZShIklqGgiSpZShIklrpzFg9MSVZA9w7hofcGfjpGB5vkOzL5sm+bH4mSz+g9748p6pG/DzjCR0KYy3JrVW1aNB1jAX7snmyL5ufydIPGJu+ePlIktQyFCRJLUPhyc4bdAFjyL5snuzL5mey9APGoC/eU5AktRwpSJJahoIkqTVlQyHJBUl+kuS7XW2nJvmPJCubr0MHWWMvksxNcl2SVUnuSLKsad8xydVJftB832HQtY5mI32ZiOdlRpJbknyn6csHmvb5SW5uzsulSZ456FpHs5G+fDrJj7rOy8JB19qrJFsk+XaSLzavJ9x5GTZCXzbpvEzZUAA+DRwyQvvHq2ph8/Xlca7p6VgLvLOq9gAWAyck2RN4D3BtVS0Arm1eb+421BeYeOflMeCAqnohsBA4JMli4KN0+rIA+BnwlgHW2KsN9QXg5K7zsnJwJT5ly4BVXa8n4nkZtm5fYBPOy5QNhar6V+A/B13HpqqqB6pqRbP8CJ1/HLOBw4ELm80uBI4YTIW920hfJpzq+EXzclrzVcABwD837RPlvGyoLxNSkjnAa4B/aF6HCXheYP2+jIUpGwob8Y4ktzWXlzb7Sy7dkswD9gFuBp5dVQ9A55ct8KzBVfbUrdMXmIDnpRnWrwR+AlwN/Dvw86pa22wyxAQJvXX7UlXD5+W05rx8PMn0AZb4VJwJ/AXwRPN6JyboeWH9vgx72ufFUHiyc4DfozNEfgD434Mtp3dJtgUuB06sqocHXc+mGKEvE/K8VNXjVbUQmAPsD+wx0mbjW9XTs25fkuwFvBd4HvAiYEfg3QMssSdJXgv8pKqWdzePsOlmf1420BfYxPNiKHSpqh83//ifAP6ezv/Im70k0+j8Er24qq5omn+cZJdm/S50/sLb7I3Ul4l6XoZV1c+Bb9K5T7J9ki2bVXOA1YOq6+no6sshzeW+qqrHgE8xMc7LEuCwJPcAn6Nz2ehMJuZ5Wa8vSf5xU8+LodBl+Jdo40jguxvadnPRXA89H1hVVWd0rboKOK5ZPg74/HjX9lRtqC8T9LzMSrJ9s7wV8Co690iuA45qNpso52WkvtzV9UdH6FyD3+zPS1W9t6rmVNU84BjgG1X1BibgedlAX964qedly9E3mZySXAK8Etg5yRDwV8Arm8e3CrgH+J8DK7B3S4A3Abc313wBTgE+AlyW5C3AfcDRA6rvqdhQX46dgOdlF+DCJFvQ+ePrsqr6YpI7gc8l+RDwbTohuLnbUF++kWQWncsvK4G3DbLITfRuJt552ZCLN+W8OM2FJKnl5SNJUstQkCS1DAVJUstQkCS1DAVJUstQ0ISV5PFmFsjvJvnC8LP0m4tmtsqjRt9yxH0XjjYbbJJPNLPH+v+xxoz/mDSR/aqZBXIvOpMbnjDogsbQQmCDodAEwZHA/cDLx6soTX6GgiaLG+maxCzJyUm+1UwKNjz//zZJvtR8LsB3k7y+ab8nyUebzwy4Jclzm/bnJLm2Oca1SXZr2j+d5Kwk/5bkh8OjgXScneTOJF+iaxLCJPsluT7J8iRf63rX6Te7fvb3k/xBOnP5/zXw+mYk9PoR+vuHdN6peg5wbNfPmZXO52esSPJ3Se5NsnOz7o3Nz1nZrNti7P7za7IwFDThNb/cDqQztQdJDgIW0JnzZSGwX5KX0/n8jNVV9cJmdPHVrsM8XFX7A2fTmQuHZvkzVbU3cDFwVtf2uwAvA15L593j0PnLfXfgBcBbgZc29UwD/hY4qqr2Ay4ATus61pbNzz4R+Kuq+jXwl8ClzUjo0hG6fSxwCXAl8NrmZ0DnnfnfqKp9m3XDQbYH8HpgSTOx3ePAGzb8X1VTlaGgiWyrZjqMB+nMBnl1035Q8/VtYAWdGSMXALcDr2r+Mv+Dqnqo61iXdH1/SbP8EuCzzfJFdEJg2L9U1RNVdSfw7Kbt5cAlzeR9q4FvNO27A3sBVzf1vp/OpGvDhicxXA7MG63TzUji0KaGh+lML35Qs/pldCZHo6q+SucDY6ATmvsB32pqOBD43dF+lqaeKTv3kSaFX1XVwiTbAV+kc0/hLDpzvny4qv5u3R2S7EfnF+qHk3y9qv66WdU938uG5n7pbn+s+7Cj7Bvgjqp6yQjruo/1OL39P3kIsB2dOaIAtgZ+CXxpnVrWreHCqnpvD8fXFOZIQRNe8xf/nwHvai6jfA3443Q+l4Eks5M8K8muwC+r6h+B04F9uw7z+q7vNzbL/0Zn9knoXGq5YZRS/hU4Jp0PpNmFznV/gO8Bs5K8pKlnWpLnj3KsR4CZG1h3LPA/qmpeM0PmfOCgJFs3Nb6u+TkHAcMfSHQtcFSSZzXrdkzynFFq0BTkSEGTQlV9O8l3gGOq6qLmGvqNzV/SvwDeCDwX+FiSJ4DfAG/vOsT0JDfT+UNp+MbtnwEXJDkZWAMcP0oZV9KZn/924PvA9U1tv25uRp/VjGq2pHPf4o6NHOs64D3NpZ4PD99XaH7xH0zXTLFV9f+S3AD8N+ADwCXNzenr6Xwo0SNV9dMk7we+3jy59Bs6I6t7R+mTphhnSdWUl86HlCyqqp8OupZNlc5HLz5eVWubkck5zY1lqSeOFKTJZTc6n6PxDODXdJ6CknrmSEGS1PJGsySpZShIklqGgiSpZShIklqGgiSp9f8BxNCzPMjtImkAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "hist_age_r = thinkstats2.Hist(resp.age_r, label='age_r')\n",
    "thinkplot.Hist(hist_age_r)\n",
    "thinkplot.Config(xlabel='Respondent Age', ylabel='Count', ylim=(190,300))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Make a histogram of <tt>numfmhh</tt>, the number of people in the respondent's household."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "hist_numfmhh = thinkstats2.Hist(resp.numfmhh, label='numfmhh')\n",
    "thinkplot.Hist(hist_numfmhh)\n",
    "thinkplot.Config(xlabel='Number of People in Household', ylabel='Count')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Make a histogram of <tt>parity</tt>, the number of children borne by the respondent.  How would you describe this distribution?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "parity = np.floor(resp.parity)\n",
    "hist_parity = thinkstats2.Hist(parity, label='parity')\n",
    "thinkplot.Hist(hist_parity)\n",
    "thinkplot.Config(xlabel='Number of Children borne by the respondent', ylabel='Count', xlim=(-1,10))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "From this histogram, we can see that the most common number of children borne by a respondent is 0, meaning that the most common result of a respondent's pregnancy did not result in a birth of a living child.\n",
    "\n",
    "This distribution can be described as a right-skewed distribution."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Use Hist.Largest to find the largest values of <tt>parity</tt>."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "22.0 1\n",
      "16.0 1\n",
      "10.0 3\n",
      "9.0 2\n",
      "8.0 8\n",
      "7.0 15\n",
      "6.0 29\n",
      "5.0 95\n",
      "4.0 309\n",
      "3.0 828\n"
     ]
    }
   ],
   "source": [
    "for parity, freq in hist_parity.Largest(10):\n",
    "    print(parity, freq)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's investigate whether people with higher income have higher parity.  Keep in mind that in this study, we are observing different people at different times during their lives, so this data is not the best choice for answering this question.  But for now let's take it at face value.\n",
    "\n",
    "Use <tt>totincr</tt> to select the respondents with the highest income (level 14).  Plot the histogram of <tt>parity</tt> for just the high income respondents."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAYUAAAEGCAYAAACKB4k+AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjMsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+AADFEAAAZ3ElEQVR4nO3dfZRU9Z3n8fcXUUF5ioCsikljJGBcBBEJK2LAp6MRojsrGkwUHCMmPkTN5Mw4Y07WTXSOmWRCTJzE0WiALHHwIRldx8mMi5IHmagoiiiIiB3txVGChkAiCeB3/6jb1wYaqDZUV9O8X+fU6Xt/9at7v13dXZ/+3Vv1u5GZSJIE0KXeBUiSOg5DQZJUMhQkSSVDQZJUMhQkSaWu9S7gT9GvX79saGiodxmStFt58sknf52Z/Vu7b7cOhYaGBhYuXFjvMiRptxIRv9refR4+kiSVDAVJUslQkCSVdutzCpLetXHjRpqamtiwYUO9S1EH0a1bNwYOHMjee+9d9WMMBamTaGpqomfPnjQ0NBAR9S5HdZaZrFmzhqamJgYNGlT14zx8JHUSGzZsoG/fvgaCAIgI+vbt2+aRo6EgdSIGglp6L78PhoIkqeQ5BamTuvz6O3fp9m7+4pRduj11THtsKOzqP5i28I9LnVVjYyMTJ05kyZIlW7R/6Utf4oQTTuDkk0/e7mOvu+46evTowRe+8IWq9nXcccexYMGCP6ne3UHz99nY2MiCBQs477zzaro/Dx9Jqrkvf/nLOwyE96KzB8LmzZuBd7/PxsZGfvjDH9Z8v4aCpF1q8+bNXHzxxRx55JGceuqpvP3220ybNo177rkHgAcffJChQ4dy/PHH87nPfY6JEyeWj33++ecZP348hx12GN/61rd2uJ8ePXoAMH/+fMaPH8/ZZ5/N0KFD+eQnP0nzZYafeOIJjjvuOIYPH87o0aNZt24dGzZs4MILL2TYsGEcffTRPPLIIwDMnDmTs846i0mTJjFo0CBuvvlmvvGNb3D00UczZswY3nzzTQBeeuklTjvtNI455hjGjRvHsmXLtlvjtGnT+MxnPsO4ceP40Ic+xAMPPABUXuDHjRvHyJEjGTlyZPnCP3/+fCZMmMB5553HsGHDtvg+r7nmGn7+858zYsQIZsyYwbhx43j66afLfY0dO5bFixdX+VPavpoePoqIRmAdsBnYlJmjIuIAYC7QADQC52TmW1E5TX4T8DHg98C0zHyqlvVJ2vVefPFF7rzzTm677TbOOecc7r333vK+DRs2cMkll/Czn/2MQYMGMWXKlodSly1bxiOPPMK6desYMmQIn/3sZ6v64NWiRYt47rnnOPjggxk7diyPPvooo0eP5txzz2Xu3Lkce+yx/Pa3v6V79+7cdNNNADz77LMsW7aMU089leXLlwOwZMkSFi1axIYNGzj88MP56le/yqJFi7j66quZPXs2V111FdOnT+eWW25h8ODBPPbYY1x66aU8/PDD262tsbGRn/70p7z00ktMmDCBFStWcOCBB/LQQw/RrVs3XnzxRaZMmVJO7vn444+zZMmSbT5bcOONN/L1r3+9DJYDDjiAmTNn8s1vfpPly5fzhz/8gaOOOqqKn9COtcdIYUJmjsjMUcX6NcC8zBwMzCvWAU4HBhe36cB326E2SbvYoEGDGDFiBADHHHMMjY2N5X3Lli3jsMMOK1/wtg6FM844g3333Zd+/fpx4IEH8vrrr1e1z9GjRzNw4EC6dOnCiBEjaGxs5IUXXuCggw7i2GOPBaBXr1507dqVX/ziF5x//vkADB06lA984ANlKEyYMIGePXvSv39/evfuzaRJkwAYNmwYjY2NrF+/ngULFjB58mRGjBjBJZdcwmuvvbbD2s455xy6dOnC4MGDOeyww1i2bBkbN27k4osvZtiwYUyePJnnn39+i++lmg+bTZ48mQceeICNGzdyxx13MG3atKqeq52px4nmM4HxxfIsYD7wV0X77KyM+34ZEX0i4qDM3PEzLqlD2Xfffcvlvfbai7fffrtcbz6sU+1jN23a9J72uWnTJjKz1ffp76iGltvp0qVLud6lSxc2bdrEO++8Q58+fbY4bLMzW9cQEcyYMYMBAwbwzDPP8M4779CtW7fy/v3337+q7e63336ccsop3Hfffdx111277DICtQ6FBP49IhL4x8y8FRjQ/EKfma9FxIFF30OAV1s8tqlo2yIUImI6lZEE73//+2tcvrT76ojvchs6dCgrV66ksbGRhoYG5s6dW9N9rVq1iieeeIJjjz2WdevW0b17d0444QTmzJnDiSeeyPLly3nllVcYMmQITz2186PVvXr1YtCgQdx9991MnjyZzGTx4sUMHz58u4+5++67mTp1Ki+/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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "high_income = resp[resp.totincr==14]\n",
    "other_income = resp[resp.totincr!=14]\n",
    "high_par = np.floor(high_income.parity)\n",
    "hist_inc_par = thinkstats2.Hist(high_par, label='high_income_parity')\n",
    "thinkplot.Hist(hist_inc_par)\n",
    "thinkplot.Config(xlabel='High Income Parity', ylabel='count')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Find the largest parities for high income respondents."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "8.0 1\n",
      "7.0 1\n",
      "5.0 5\n",
      "4.0 19\n",
      "3.0 123\n"
     ]
    }
   ],
   "source": [
    "for parity, freq in hist_inc_par.Largest(5):\n",
    "    print(parity, freq)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Compare the mean <tt>parity</tt> for high income respondents and others."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Mean Parity for High Income Respondents: 1.0758620689655172\n",
      "Mean Parity for Other Income Respondents: 1.2495758136665125\n"
     ]
    }
   ],
   "source": [
    "print('Mean Parity for High Income Respondents:',high_income.parity.mean())\n",
    "print('Mean Parity for Other Income Respondents:',other_income.parity.mean())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Compute the Cohen effect size for this difference.  How does it compare with the difference in pregnancy length for first babies and others?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "-0.1251185531466061"
      ]
     },
     "execution_count": 40,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "CohenEffectSize(high_income.parity,other_income.parity)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "There is a much larger difference between the parity of high income households and lower income households than there is a difference between pregnancy lengths for first born babies and others. This is to say that the higher a household income, the fewer the children birthed by a mother in that household. \n",
    "\n",
    "The relationship of income to parity was much stronger than the relationship of pregnancy length to first babies."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {},
   "outputs": [],
   "source": [
    "# exercise 2.3: function for mode of hist that returns most freq value:\n",
    "def Mode(Hist):\n",
    "    mode = 0\n",
    "    for val in sorted(hist.Values()):\n",
    "        if hist[val] > mode:\n",
    "            mode = val\n",
    "    return mode\n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2"
      ]
     },
     "execution_count": 57,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "Mode(hist_inc_par)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Exercise 2.4: \n",
    "Investigate whether first babies are lighter or heavier than others. Compute Cohen's *d* to quantify the difference between the groups. how does it compare to the difference in pregnancy length?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [],
   "source": [
    "firsts = live[live.birthord == 1]\n",
    "others = live[live.birthord != 1]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "7.201094430437772\n",
      "7.325855614973262\n"
     ]
    }
   ],
   "source": [
    "print(firsts.totalwgt_lb.mean())\n",
    "print(others.totalwgt_lb.mean())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 60,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "-0.088672927072602"
      ]
     },
     "execution_count": 60,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 62,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "38.60095173351461\n",
      "38.52291446673706\n"
     ]
    }
   ],
   "source": [
    "print(firsts.prglngth.mean())\n",
    "print(others.prglngth.mean())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.028879044654449883"
      ]
     },
     "execution_count": 64,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "CohenEffectSize(firsts.prglngth, others.prglngth)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "There is a much stronger correlation between birth order and total weight than there is a correlation between birth order and pregnancy length. Based on this data, we can conclude that although there is not a strong correlation of first born babies being born later, there is a correlation of first born babies weighing less at birth than proceeding babies."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.6"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 1
}
