diff l norms
diff privacy with not laplace but gaussian

![[Pasted image 20260702181242.png]]


FGSM
```python
class FGSM(nn.Module):

def __init__(self, model, eps=0.3, criterion=nn.CrossEntropyLoss(),device='cpu'):
super().__init__()
self.model = model
self.eps = eps
self.criterion = criterion
self.device = device

def forward(self, images, labels):

# Prepare data
images = images.clone().detach().to(self.device)
labels = labels.clone().detach().to(self.device)

# Specify that the gradients need to be computed
images.requires_grad = True

# Generate predictions
predictions = self.model(images)

# Compute Loss
loss = self.criterion(predictions,labels)

# Calculate the derivative of the loss w.r.t. the original image
grad = torch.autograd.grad(loss, images, retain_graph=False, create_graph=False)[0]

# Compute the adversarial images
adv_images = images + self.eps*grad.sign()
adv_images = torch.clamp(adv_images, min=0, max=1).detach()

return adv_images
```


PGD
```python
class PGD(nn.Module):

def __init__(self, model, eps=8/255,

alpha=2/255, steps=10, random_start=True,loss=nn.CrossEntropyLoss()):

super().__init__()

self.model = model

self.eps = eps

self.alpha = alpha

self.steps = steps

self.random_start = random_start

self.loss = loss

@property

def device(self):

return next(self.model.parameters()).device

  

def forward(self, images, labels):

images = images.clone().detach().to(self.device)

labels = labels.clone().detach().to(self.device)

  

adv_images = images.clone().detach().to(self.device)

  

if self.random_start:

# Starting at a uniformly random point

adv_images = adv_images + torch.empty_like(adv_images).uniform_(-self.eps, self.eps)

adv_images = torch.clamp(adv_images, min=0, max=1).detach()

# Implementation of PGD

# PGD is a multi-step version of FGSM

for _ in range(self.steps):

#prepare inputs

adv_images.requires_grad = True

#get the predictions of the inputs

outputs = self.model(adv_images)

  

# Calculate loss

cost = self.loss(outputs, labels).to(device)

self.model.zero_grad()

cost.backward()

  

grad = adv_images.grad.detach()

# Update adversarial images

adv_images = adv_images.detach() + self.alpha*grad.sign()

#project x_t back by torch.clamp

eta = torch.clamp(adv_images - images, min=-self.eps, max=self.eps)

adv_images = torch.clamp(images + eta, min=0, max=1).detach().to(self.device)

return adv_images
```

Backdoor Attack
```python
class GenerateSQRTrigger:

"""

A class that creates a random square pattern that is used as a trigger for an

image dataset.

"""

  

def __init__(self, size, pos_label, dataset='mnist'):

  

datasets_dimensions = {"mnist": (28, 28, 1),

"cifar10": (32, 32, 3),

"fmnist": (28, 28, 1)}

  

dims = datasets_dimensions[dataset]

  

if size[0] != size[1]:

raise Exception("The size of the trigger must be square.")

  

if pos_label.lower() not in ["upper-left", "upper-mid", "upper-right", "mid-left", "mid-mid", "mid-right",

"lower-left",

"lower-mid", "lower-right"]:

raise Exception(

"The position of the trigger must be one of the following: upper-left, upper-mid, upper-right, mid-left, mid-mid, mid-right, lower-left, lower-mid, lower-right")

  

if size[0] > dims[0] or size[1] > dims[1]:

raise Exception("The size of the trigger is too large for the dataset items.")

  

self.dims = dims

self.size = size

self.pos_label = pos_label

# pos == position; coordinates

self.pos_coords = self._gen_pos_square()

  

trigger = np.zeros(self.dims, dtype=np.float32)

self.crafted_trigger = self.create_trigger_square(trigger)

  

def _gen_pos_square(self):

if self.pos_label == "upper-left":

return (0, 0)

elif self.pos_label == "upper-mid":

return (0, self.dims[1] // 2 - self.size[1] // 2)

elif self.pos_label == "upper-right":

return (0, self.dims[1] - self.size[1])

  

elif self.pos_label == "mid-left":

return (self.dims[0] // 2 - self.size[0] // 2, 0)

elif self.pos_label == "mid-mid":

return (self.dims[0] // 2 - self.size[0] // 2,

self.dims[1] // 2 - self.size[1] // 2)

elif self.pos_label == "mid-right":

return (self.dims[0] // 2 - self.size[0] // 2, self.dims[1] - self.size[1])

  

elif self.pos_label == "lower-left":

return (self.dims[0] - self.size[0], 0)

elif self.pos_label == "lower-mid":

return (self.dims[0] - self.size[0], self.dims[1] // 2 - self.size[1] // 2)

elif self.pos_label == "lower-right":

return (self.dims[0] - self.size[0], self.dims[1] - self.size[1])

  

def create_trigger_square(self, trigger):

"""Create a square trigger."""

base_x, base_y = self.pos_coords

for x in range(self.size[0]):

for y in range(self.size[1]):

trigger[base_x + x][base_y + y] = \

np.ones((self.dims[2]))

  

return trigger

  

def apply_trigger(self, img):

"""applies the trigger on the image."""

  

base_x, base_y = self.pos_coords

for x in range(self.size[0]):

for y in range(self.size[1]):

img[base_x + x][base_y + y] = self.crafted_trigger[base_x + x][base_y + y]

return img
```