## Neuron
[NEURON](https://www.neuron.yale.edu/neuron/) is a simulation environment for modeling individual neurons and networks of neurons.

## Why we use "Neuron"??
It is not only easy simulation tool, but also empirically based.

Many neuroscientist use Neuron, and it has nice model zoo (modelDB)


## ModelDB: computational neuroscience models
https://senselab.med.yale.edu/ModelDB/

## NEURON Python documentation
https://www.neuron.yale.edu/neuron/static/py_doc/index.html

## The Neuron Book
[Guide Book](https://www.amazon.com/NEURON-Book-Nicholas-T-Carnevale/dp/0521115639/ref=sr_1_1?s=books&ie=UTF8&qid=1536419942&sr=1-1&keywords=the+neuron+book)

<img src="https://user-images.githubusercontent.com/19909579/45587170-2a61db80-b93d-11e8-9caa-0abb602a2004.jpg" width="30%">

## demo

```
begintemplate SThcell
public soma, treeA, treeB ,nclist

create soma, treeA[1], treeB[1]
objectvar f, nclist

proc init() {local i, me, child1, child2 

    access soma

//set parameters for soma section
    nseg=1
    diam=18.8
    L = 18.8
    Ra=123.0

//add predefined channel parameters called
//hh - Hodgkin Huxley and pas - passive potassium and sodium
    insert hh
    gnabar_hh= 0.2
    gl_hh = .0001666
    el_hh = -60.0

    insert pas

    nclist = new List()


//load treeA
    f = new File()
    f.ropen("treeA.dat")
    ndendA = f.scanvar()
    create treeA[ndendA]

//define treeA

    for i = 0,ndendA-1 {
      me = f.scanvar() - 1
      child1 = f.scanvar() - 1
      child2 = f.scanvar() - 1

      treeA[me] {
        nseg = 1
        diam = f.scanvar()
        L = f.scanvar()
        Ra = 123

        insert pas
        g_pas = .0001666
        e_pas = -60.0

	// initialise and clear the 3D information
        pt3dclear()
        pt3dadd(f.scanvar(),f.scanvar(),f.scanvar(),diam)
        pt3dadd(f.scanvar(),f.scanvar(),f.scanvar(),diam)

        if (child1 >= 0) {
          connect treeA[child1](0), 1
        }
        if (child2 >= 0) {
          connect treeA[child2](0), 1
        }
      }
    }
f.close

//treeB

f = new File()
    f.ropen("treeB.dat")
    ndendB = f.scanvar()
    create treeB[ndendB]

    for i = 0,ndendB-1 {
      me = f.scanvar() - 1
      child1 = f.scanvar() - 1
      child2 = f.scanvar() - 1

      treeB[me] {
        nseg = 1
        diam = f.scanvar()
        L = f.scanvar()
        Ra = 123

        insert pas
        g_pas = .0001666
        e_pas = -60.0

	// initialise and clear the 3D information
        pt3dclear()
        pt3dadd(f.scanvar(),f.scanvar(),f.scanvar(),diam)
        pt3dadd(f.scanvar(),f.scanvar(),f.scanvar(),diam)

        if (child1 >= 0) {
          connect treeB[child1](0), 1
        }
        if (child2 >= 0) {
          connect treeB[child2](0), 1
        }
      }
    }
	f.close

    // Connect things to the soma
    connect treeA[0](0), soma(1)
    connect treeB[0](0), soma(0)
}
endtemplate SThcell







//create neuron from templete
nSThcells = 4
objectvar SThcells[nSThcells]

SThcells[0] = new SThcell()
SThcells[1] = new SThcell()
SThcells[2] = new SThcell()
SThcells[3] = new SThcell()

//graph default
access SThcells[0].soma



//insert current clamps

objectvar stim[nSThcells]


//SThcells[3] to activate SThcells[0]
i = 3
 SThcells[i].soma {
    stim[i] = new IClamp(0.5)
    stim[i].del = 100
    stim[i].dur = 100
    stim[i].amp = 0.45
}
maxsyn = 10
objectvar syn[maxsyn]



//connect SThcells[1] to the end of treeA dendritic branch 22 on SThcells[0]
access SThcells[0].treeA[22]
syn[0] = new ExpSyn(0)
access SThcells[1].soma
SThcells[0].nclist.append(new NetCon(&v(1), syn[0], 10, 1, 0.5))


//connect SThcells[2] to the end of treeA dendritic branch 22 on SThcells[1]
access SThcells[1].treeA[22]
syn[1] = new ExpSyn(0)
access SThcells[2].soma
SThcells[1].nclist.append(new NetCon(&v(1), syn[1], 10, 1, 0.5))

//connect SThcells[3] to the end of treeA dendritic branch 22 on SThcells[2]
access SThcells[2].treeA[22]
syn[2] = new ExpSyn(0)
access SThcells[3].soma
SThcells[2].nclist.append(new NetCon(&v(1), syn[2], 10, 1, 0.5))

//connect SThcells[3] to the end of treeA dendritic branch 21 on SThcells[2]
access SThcells[2].treeA[21]
syn[3] = new ExpSyn(0)
access SThcells[3].soma
SThcells[2].nclist.append(new NetCon(&v(1), syn[3], 10, 1, 0.5))


//connect SThcells[3] to the end of treeA dendritic branch 20 on SThcells[2]
access SThcells[2].treeA[20]
syn[4] = new ExpSyn(0)
access SThcells[3].soma
SThcells[2].nclist.append(new NetCon(&v(1), syn[4], 10, 1, 0.5))


//connect SThcells[3] to the end of treeA dendritic branch 19 on SThcells[2]
access SThcells[2].treeA[19]
syn[5] = new ExpSyn(0)
access SThcells[3].soma
SThcells[2].nclist.append(new NetCon(&v(1), syn[5], 10, 1, 0.5))


//connect SThcells[3] to the end of treeA dendritic branch 18 on SThcells[2]
access SThcells[2].treeA[18]
syn[6] = new ExpSyn(0)
access SThcells[3].soma
SThcells[2].nclist.append(new NetCon(&v(1), syn[6], 10, 1, 0.5))

print v
forall psection()
tstop=300
```
