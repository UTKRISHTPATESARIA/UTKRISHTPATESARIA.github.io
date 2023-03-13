# IISc CSA Mtech Research Interview

## The Interview

Prior to the interview the google forms were sent out to record our area of interests and sub-domains.

AIR: 141
Score: 815
Interview Date: 24th May 2 PM.
Venue: CSA department.

This year number of applicants for Computing Systems was very less.
I was called up first for my interview.

**Panel : (Prof.) Arkaprava Basu, Vinod Ganapathy, KV Raghavan, RC Handsah, Deepak D’souza.**

I am splitting my interview into 3 parts so that it will be easier to read. Please ignore part one as it was completely related to my job experience, though you can definitely read up and get to know a lot of things. Second part was basics on OS and third part was coding.

### Part - I

RH: He read out my profile and checked it with me. So you passed out from KIIT in 2020, and after that you have been working at NUTANIX!!. He asked me to describe my job profile and what I have been working on.

*(The next 20 mins of the interview was completely based on my job experience and the level of interview got escalated Please don’t expect these kind of questions as the interview was very specific to my job role).*

AB : So you have worked at Nutanix, describe any one problem that you have worked on during your tenure that you found interesting.

Me: *(Coming to my role at Nutanix, it was a troubleshooting job and I have worked over 625+ issues, I chose a random one)*. Firstly I described the whole Nutanix distributed stack to them, the hardware configuration, explained them a cluster having 4 nodes, with each of them having Type-I hypervisor installed (Hyper-V / ESXi / AHV).

AB : Okay stop for a min. Explain me what are hypervisors and different types.

Me: I explained them the concept of virtualisation and why we need them and how hypervisors help us achieving the above. Gave them examples of bare metal and hosted hypervisors. Explained about Oracle Virtual BOX & VMware fusion working as Type II. I explained them that AHV is Nutanix custom Type-I hypervisor (modified KVM).

AB : Are you sure KVM is Type-I?

Me : I know XEN is para-virtualised and KVM is the next upgrade to it.

AB : *(He corrected me and explained me how the VM’s running on KVM each is a process (VMM’s). I think they have done some modifications in AHV to make it Type - I)*. No problem , let’s continue with your tshoot scenario.

*(Had some discussion on I/O path from VM to disk and also on PCI passthrough and how is it achieved)*

Me : On the architecture that I built, I took an example where let’s suppose this is a critical clusters with multiple VM’s running on every node. Suppose due to some hardware failure 1 node goes down. HA will kick-in and try to restart the VM’s on the down node on some other working node. But due to resource contention we are not able to. It’s a bank cluster and every minute they are loosing transactions worth of 10k+ $. 
I prioritized the issue into 3 parts:

1. Get the VM running
2. Get the node running
3. Root cause the issue.

AB : Sounds good, tell me how will you get the VM running?

Me : I took an example in the scenario where no node in the cluster has sufficient free memory that can sustain the down VM’s memory config. 
Scenarios:

1. Try to check if there are some non-critical VMs that can be turned down to accommodate this critical VM.

AB : Wait, why will you turn down the VM first. Think of a better solution.

Me : Yes, we can migrate running VM’s in such a way that 1 node has enough space to accommodate the critical VM.

AB : Yes I was looking for that. Okay so just one follow-up question then we will move on to the  basics.

VG : Do you know Live migration?

Me : Yes.

VG : Can you explain the process?

Me : Took a scenario where there is a VM1 on Node 1 and we have to live migrate it to Node 2. So before migrating we need to take care of few parameters associated with a VM - vCPUs, RAM, Disk/Storage. 

AB : You forgot to add network. :)

Me : Ahh yes. I explained them about EVC, where we need to make sure the CPU architecture compatibility of 2 nodes before migration.
So suppose there is a write operation ongoing when the request for live migration was initiated.
Lets say a process P1 is continuously performing writes. Now every process has a page table keeping track of pages. We will first migrate all the non-dirty pages and then implement quescing/freezing to prevent further writes in such a way that there will be no impact on running VM.

AB : Okay, how will you implement freezing of writes? *(The other professors said let’s move on to basics but Arka sir insisted and he said I am seeing whether he can relate this to basics or not?)*

Me : (Confused!) I explained them about Vmware’s VAAI plugin and how it takes care to transfer dirty pages and resume writes at destination host.

AB : No, I want to understand how are you preventing the writes?

Me : (It clicked!) Yes, so whatever pages are dirty the specific page table entry for that page will have it’s bit set. OS will scan through page table and make that page read-only by setting the specific bit for that PTE. Hence no more writes can happen. We will maintain a queue for pending/failed writes and after the page has been transferred to the destination, the writes can be initiated again.

*Everyone seemed happy with the explanation.I think this was the part that got me through IISc.*

### Part - II

VG : Okay let’s move on to the basics. Can you write down a sample template code for how would you create a process *(they wanted to know what would happen at backend when a process is being created)*?

Me : Sure, let me try. Let me write a wrapper function and then proceed.

```
void CreateProcess() {

Initialise();
AllocateMemory();

}
```

AB : Can you draw a layout for the memory partitioning for a process, assume a C program?

Me : I drew the layout and explained it as per below.

AB : Can you write the function calls to allocate memory to heap and stack?

Me : We use sbrk() to allocate for heap and for stack I think it is the same.

AB : Are you sure?

Me : No I am not.

AB : No worries, we use mmap() for that.

Me : Ahh, yes :)

VG : Can you explain how does a page-table gets built up when a process starts executing for first time?

Me : I took an example and showed them let's assume a process is having X GB of memory, Page size of Y KB. And accordingly I built up the page tables and showed them diagrammatically.

VG : Im interested in knowing what the are contents of Page table.

Me : (*Well I messed it up!*) It contains page number along with MM frame number(actual physical address). (*I forogt to mention about all the bits for protection, dirty, R/W and how page no are related with frame number. PTE doesn't contain page number. I should have explained the whole virtual to physical address conversation)*.

(* I haven;t described this part properly but I remeber writing some psuedocode and explaining some other concepts too*)

I referred the following resources which strengthened by base before the interviews:

[Prof. Sorav Bansal IIT-D OS](https://youtube.com/playlist?list=PLTtjs-HViBW6525-_a8QL3meFIlP31gGE)

[Prof. Mythili IIT-B OS](https://youtube.com/playlist?list=PLDW872573QAb4bj0URobvQTD41IV6gRkx)

[Prof. Biswabandan Panda IIT-B Architecture](https://youtube.com/playlist?list=PLw6vmiIQrilTWa5twNV8opVJ3ge_kEfsM)

[Prof Vinod and Arka IISc - OS](https://www.csa.iisc.ac.in/~vg/teaching/E0-253/schedule.html)


### Part - III

AB : Let's move on to coding. 

**(I have read so many interview blogs and interacted with a lot of people who interviewed for same profile, from all of them I inferred that they just check whether you know basics of coding or not. Over the past few years students have been asked on Link lists, baiscs of BST and Binary trees(invert, traversals. So I prepared accordingly, but....)**

AB : Are you aware of height balanced trees?

Me : Yes I am, in a binary tree for every node the difference of left and right subtree should difer by a constant, in case of AVL the constant is 1.

AB : What is special about AVL?

Me : Along with it being height balanced it is also a BST.

AB : Why do we use height balanced trees?

Me : Improvising search time to O(logn). Explained them about skewed trees and disadvantages.

AB : Sounds good. So can you write down a code in C to check whether the given tree is AVL or not?

Me : *(It was already 40 mins into my interview and I was like will they ask everything from me only? Coming to the code I wrote down the basic template and messed up the logic in the first attempt(top-down approach). Now Arka sir started pressurizing, I asked him for few mins to help me correct my logic. To calm myself down I wrote the code for finding height of the tree as it was required. After that I got a bit of confidence and fortunately it struck. I corrected the logic(Bottom-up approach) and took an example to run the code and it worked!!)*

The code:

```
int height(Node *root){

  if(root == NULL) return 0;

  int l = height(root -> left);
  int r = height(root -> right);

  return l > r ? l + 1 : r + 1;
}

int checkAVL(Node *root){

  if(root == NULL)
    return 1;
    
  //Bottom UP approach, check from leaves and then propogate up to the root.
  
  int a = check_AVL(root -> left);
  int b = check_AVL(root -> right);
  
  if(a && b){
    
    int l = height(root -> left);
    int r = height(root -> right);
    
    if( abs(l - r) <= 1)
      return 1;
    else
      return 0;
  }
  else return 0;
}

```

AB : Okay one last question before we wrap up. What is the differnce between process and thread.

Me : I explained him with the help of diagram and he seem satisfied. 




