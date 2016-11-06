# JSON
ViewController.m
#import "ViewController.h"
#import "CollectionView.h"

@interface ViewController ()
{
    NSString *str;
    NSData *data3;
    NSURL *url;
    NSURL *imgurl;
    NSURLRequest *urlreq;
    NSURLConnection *urlconnect;
    NSMutableData *data1;
    NSDictionary *dict;
    NSArray *arr;
    NSMutableArray *datalast;
}

@end

@implementation ViewController

- (void)viewDidLoad {
    
    data1=[[NSMutableData alloc]init];
    datalast=[[NSMutableArray alloc]init];
    str=@"https://foodpalette.in/Chennai/Api/UserHomeApi.php";
    url=[NSURL URLWithString:str];
    urlreq=[[NSURLRequest alloc]initWithURL:url];
    urlconnect=[[NSURLConnection alloc]initWithRequest:urlreq delegate:self];
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
}
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response
{
    data1=[NSMutableData data];
    
}

- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data

{
    [data1 appendData:data];
    
}
- (void)connectionDidFinishLoading:(NSURLConnection *)connection
{
    
    
    dict = [NSJSONSerialization JSONObjectWithData:data1 options:NSJSONReadingAllowFragments error:nil];
    
    
   
    NSLog(@"arr-->%@",[[[dict valueForKey:@"ourpicks"]valueForKey:@"ProductName"]objectAtIndex:3]);
     [collect reloadData];
}
- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
{
    
    return [[dict valueForKey:@"ourpicks"]count ];
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
{
    CollectionView *cellObj=[collect dequeueReusableCellWithReuseIdentifier:@"cell" forIndexPath:indexPath];

    NSString * str2 = [[[dict valueForKey:@"ourpicks"]valueForKey:@"ProductImage"]objectAtIndex:indexPath.row];
    url=[NSURL URLWithString:str2];
    NSData *data2=[NSData dataWithContentsOfURL:url];
    cellObj.img.image=[UIImage imageWithData:data2];
    cellObj.lab.text = [[[dict valueForKey:@"ourpicks"]valueForKey:@"ProductName"]objectAtIndex:indexPath.row];
    

    return cellObj;

}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
