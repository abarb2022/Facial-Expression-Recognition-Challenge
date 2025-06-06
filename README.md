# Facial-Expression-Recognition-Challenge

## პროექტის მიმოხილვა
ამ პროექტში ვქმნით კონვოლუციურ ნეირონულ ქსელებს (CNN), რომლებიც ამოიცნობენ 7 ემოციას (ბრაზი, ზიზღი, შიში, ბედნიერება, სევდა, გაკვირვება და ნეიტრალური) 48×48 ზომის  სურათებზე. 
ჩემმა საუკეთესო მოდელმა მიაღწია 64% accuracy-ს ვალიდაციის set-ზე.

## ჩემი მიდგომა
ვცადე 4 სხვადასხვა CNN არქიტექტურა, ყველა მოდელში დატას ვყოფ სამ ნაწილად  (train/val (PublicTest)/test (PrivateTest)), ვანორმალიზებ პიქსელებს [0, 255] → [0, 1] , Reshaped -> 48×48×1 (height × width × channels)


## ფაილები
simple_model.ipynb - მარტივი 3-შრიანი არქიტექტურა

model_2.ipynb - რეგულარიზებული ვერსია BatchNorm-ით და dropout-ით

model_3.ipynb -  შედარებით ღრმა ქსელი მეტი layer-ით

model_4.ipynb 


## მოდელები

### მოდელი 1: მარტივი CNN
3 Conv2d შრე (32→64→128 ფილტრი)
Linear(4608→512→7) 
ამ მოდელში ვუყენებ მხოლოდ ერთ Dropout შრეს (p=0.5), ოპტიმიზატორად: Adam (lr=0.001), loss: CrossEntropyLoss, არ არის BatchNorm შრეები და არც L2 რეგულარიზაცია.

შედეგები

train_acc: 75.99%

val_acc: 55.70%

test_acc: 57.06%

train-ის accuracy მაღალია ვალიდაციასა და ტესტთან შედარებით, გვაქვს overfit. მოდელმა დაიწყო overfit-ისკენ წასვლა 10 epoch-ის შემდეგ, 10-დან train_acc იზრდებოდა, რადგან უკვე იზეპირებდა, ხოლო ვალიდაცია დარჩა ისე რადგან ასეთი მარტივი მოდელი მეტად განვითარების საშუალებას არ იძლევა. 


### მოდელი 2: 
გაუმჯობესებები:
დავამატე BatchNorm ყველა კონვოლუციურ შრეს, დავამატე Dropout2d (0.25) კონვოლუციურ შრეებში და  weight decay (L2 რეგულარიზაცია)

შედეგები

test_acc	0.61772

train_acc	0.90526

val_acc	0.6124


კვლავ წავიდა overfit-ში, dropout 0.25 არ იყო საკმარისი ღრმა შრეებისთვის.

### მოდელი 3: ღრმა ქსელი
გაუმჯობესებები
დავამატე 2 კონულუციური შრე ყოველ ბლოკში, ღრმა კლასიფიკატორი BatchNorm-ით , ფილტრების რაოდენობა გავზარდე, აგრესიული dropout (0.3-0.5)
ამ მოდელმა აჩვენა ყველაზე ნაკლები overfit და ყველაზე კარგი შედეგები, რაც ალბათ დამატებითმა კონუცუციურმა შრეებმა განაპირობა:

test_acc: 0.6213

train_acc: 0.6398

val_acc: 0.615

ასევე, ამ მოდელში მთლიან დატაზე დატრენინგებმადე პატარა დატაზე ვცადე მოდელის დატრენინგება, train_acc 1.0 მივიღე ბოლოს, ანუ წარმატებით დაოვერფიტდა, რაც კარგი შედეგია და მიუთითებს, რომ მოდელს შეუძლია ისწავლოს სრულყოფილად, როცა მონაცემები საკმარისია.



### მოდელი 4: 
ეს მოდელი გავს ძალიან მეორეს, უბრალოდ აქ maxpooling შევცვალე Hybrid Pooling (Avg + Max Pool)-ით, შედეგები ოდნავ შეიცვალა

val_acc : 0.643

train_acc: 0.99

test_acc: 0.653

ყველაზე დიდი ოვერფიტი ამ ცვლილებამ გამოიწვია.
საბოლოოდ, საუკეთესო შედეგები მესამამე არქიტექურამ მოგვცა


### MLflow tracking
https://wandb.ai/abarb2022-free-university-of-tbilisi-/facial-expression-recognition
