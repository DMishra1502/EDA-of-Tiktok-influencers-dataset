import pandas as pd
import numpy as np
import matplotlib as plt
data = pd.read_excel(r"D:\Python files\Social_Media_Influencers.xlsx")
data
data.info()
data.describe()
data.count()
data.describe(include='all')
data.duplicated()
data.drop_duplicates()
def remove_symbols_and_convert(value):
    if isinstance(value, int) or isinstance(value, float):
        return value

    multiplier = 1
    if 'k' in value.lower():
        multiplier = 1000
    elif 'm' in value.lower():
        multiplier = 1000000

    value = value.replace('k', '').replace('K', '').replace('m', '').replace('M', '')

    return float(value) * multiplier


# Apply the function to the columns where you want to remove symbols
columns_to_process = ['Subscribers','Views_Ave','Likes_Ave','Comments_Ave','Shares_Ave']  # Add the columns you want to process
for col in columns_to_process:
    data[col] = data[col].apply(remove_symbols_and_convert)

# Display the modified DataFrame
display(data)
s0=data.Subscribers.min()
s100=data.Subscribers.max()
q1=data.Subscribers.quantile(0.25)
q2=data.Subscribers.quantile(0.5)
q3=data.Subscribers.quantile(0.75)
iqr=q3-q1
lc=q1-1.5*iqr
uc=q3+1.5*iqr
lc
uc
print("s0=",s0,"s100=",s100,",lc=",lc,",uc=",uc)
data.Subscribers.plot(kind='box')
data.Subscribers.clip(upper=uc)
data.Subscribers.clip(upper=uc,inplace=True)
data.Subscribers.plot(kind='box')
data.isna()
data.isna().sum()/data.shape[0]
HS = data.loc[data['Subscribers'].idxmax()]
THS = HS['Tiktok name']
print("Ticktok with highest number of subscribers is",THS)
HV = data.loc[data['Views_Ave'].idxmax()]
THV = HV['Tiktok name']
print("Tiktoker with highest number of views on videos is",THV)
HC = data.loc[data['Comments_Ave'].idxmax()]
THC = HC['Tiktok name']
print("Tiktoker with highest number of comments on videos is",THC)
HS = data.loc[data['Shares_Ave'].idxmax()]
THS = HS['Tiktok name']
print("Tiktoker with highest number of videos shares is",THS)
data.plot(x='Subscribers',y='Views_Ave',kind='scatter')
plt
data.plot(x='Subscribers',y='Likes_Ave',kind='scatter')
plt
data.plot(x='Subscribers',y='Comments_Ave',kind='scatter')
plt
data.plot(x='Subscribers',y='Shares_Ave',kind='scatter')
plt
#Correlation matrix
#Finding correlation between all the numeric variables
data.drop('S.no',axis=1,inplace=True)
data.select_dtypes(['float64','int64']).corr()
data['Subscribers'].corr(data['Views_Ave'])
data['Subscribers'].corr(data['Likes_Ave'])
data['Subscribers'].corr(data['Comments_Ave'])
data['Subscribers'].corr(data['Shares_Ave'])
!pip install seaborn --upgrade
import seaborn as sns
sns.heatmap(data.select_dtypes(['float64','int64']).corr(),annot=True)
plt
