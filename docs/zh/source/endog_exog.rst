.. _endog_exog:

``endog``, ``exog``, what's that?
什么是``endog``, ``exog`？
=================================

Statsmodels is using ``endog`` and ``exog`` as names for the data, the
observed variables that are used in an estimation problem. Other names that
are often used in different statistical packages or text books are, for
example,

Statsmodels模型使用``endge``和``exge``作为数据的名称，其实质是在估计问题中使用的观测变量。在不同的统计数据包或教科书中经常使用的其他名称如下：

===================== ======================
endog                 exog
===================== ======================
y                     x
因变量                自变量
left hand side (左手边)  right hand side (右手边) 这个在统计学中不常这么说，你可以理解为模型中等式（=）两边的变量，下面的名称都没有进行翻译，原因是，翻译的结果都是因变量和自变量
dependent variable    independent variable
regressand            regressors
outcome               design
response variable     explanatory variable
===================== ======================


The usage is quite often domain and model specific; however, we have chosen
to use `endog` and `exog` almost exclusively. A mnenomic hint to keep the two
terms apart is that exogenous has an "x", as in x-variable, in it's name.
以上多种叫法通常是特定于领域和模型的；Statsmodels 几乎只选择使用“endog”和“exog”来表示因变量和自变量。
保持这两个术语分开的一个记忆提示是，外生变量的名称中有一个“x”，就像x变量一样。（其实对于学统计学的人来讲so easy！）

`x` and `y` are one letter names that are sometimes used for temporary
variables and are not informative in itself. To avoid one letter names we
decided to use descriptive names and settled on ``endog`` and ``exog``.
Since this has been criticized, this might change in future.
`“x”和“y”是一个字母的名称，有时用于临时变量，本身不提供信息。为了避免使用一个字母名称，或者说避免引起歧义，
Statsmodels决定沿用`` endog``和`` exog``这种描述性名称。这一点和许多领域（如统计学）的称谓脱轨（或者说不一致），
在今后可能会有所改变（可能性不大）。

Background
背景
----------

Statsmodels中一些术语的非正式的定义如下：

`endogenous`：内生变量，数据或者事物自身的因素（经济学中定义为：在模型中的变量就是内生变量）

`exogenous`：外生变量，数据或者事物自身以外的因素（经济学中定义为：是由模型以外的因素所决定的已知变量，相当于没有被模型所解释的，并且纳入模型了的变量）

*Endogenous variables designates variables in an economic/econometric model
that are explained, or predicted, by that model.*
*内生变量指经济/计量经济模型中由该模型解释或预测的变量*
http://stats.oecd.org/glossary/detail.asp?ID=794

*Exogenous variables designates variables that appear in an
economic/econometric model, but are not explained by that model (i.e. they are
taken as given by the model).*  
*外生变量是指出现在经济/计量经济模型中，但未被该模型解释的变量（即，按模型给出的值取）*
http://stats.oecd.org/glossary/detail.asp?ID=890

In econometrics and statistics the terms are defined more formally, and
different definitions of exogeneity (weak, strong, strict) are used depending
on the model. The usage in statsmodels as variable names cannot always be
interpreted in a formal sense, but tries to follow the same principle.
在计量经济学和统计学中，术语的定义更为正式，根据模型的不同，使用了不同的外生性定义（弱、强、严）。
statsmodels中作为变量名的用法不能总是在形式意义上解释，而是尝试遵循相同的原则。


In the simplest form, a model relates an observed variable, y, to another set
of variables, x, in some linear or nonlinear form ::
在最简单的模型中，一个模型将一个观测变量y与另一组变量x以某种线性或非线性形式联系起来：

   y = f(x, beta) + noise
   y = x * beta + noise

However, to have a statistical model we need additional assumptions on the
properties of the explanatory variables, x, and the noise. One standard
assumption for many basic models is that x is not correlated with the noise.
In a more general definition, x being exogenous means that we do not have to
consider how the explanatory variables in x were generated, whether by design
or by random draws from some underlying distribution, when we want to estimate
the effect or impact that x has on y, or test a hypothesis about this effect.
然而，为了建立一个统计模型，我们需要对解释变量x和噪声(残差)的性质进行额外的假设。许多基本模型的一个标准假设是x与噪声(残差)无关。
在更一般的定义中，x是外生的，这意味当我们想要估计x对y的影响的时候，我们不必考虑x中的解释变量是如何产生的，无论是通过模型设计还是通过从一些潜在分布中随机抽取。

In other words, y is *endogenous* to our model, x is *exogenous* to our model
for the estimation.
简单来说，y是你所建立的模型内生变量，x是模型的外生变量（相当于把x的来源放得更广泛）。

As an example, suppose you run an experiment and for the second session some
subjects are not available anymore.
Is the drop-out relevant for the conclusions you draw for the experiment?
In other words, can we treat the drop-out decision as exogenous for our
problem.
举个例子，假设你做了一个实验，而在第二节课中，有些科目已经不可用了（这里的原文我认为有问题，反正是举的一个例子无所谓啦）。退学与你为实验得出的结论是否有关？
换言之，我们能否将退学决定视为我们问题的外生因素。

It is up to the user to know (or to consult a text book to find out) what the
underlying statistical assumptions for the models are. As an example, ``exog``
in ``OLS`` can have lagged dependent variables if the error or noise term is
independently distributed over time (or uncorrelated over time). However, if
the error terms are autocorrelated, then OLS does not have good statistical
properties (is inconsistent) and the correct model will be ARMAX.
``statsmodels`` has functions for regression diagnostics to test whether some of
the assumptions are justified or not.
使用者需要去了解（或查阅教科书找出）模型成立的基本假设。例如，``OLS``中的``exog``可以有滞后的因变量，
如果误差或噪声项随时间独立分布（或随时间不相关）。但是，如果误差项是自相关的，则OLS不具有良好的统计特性（不一致），正确的模型将是ARMAX
``statsmodels``具有回归诊断功能，用于测试某些假设是否合理。
