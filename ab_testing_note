############################
# Sampling (Örnekleme)
############################
->numpy içerisinden random modülünden random int. Diyerek 0-80 arasında 10000 tane sayı oluşturduk. Yapmış olduğumuz bu işlem popülasyondur.
populasyon = np.random.randint(0, 80, 10000)

->10000 tane sayının ortalamasını aldık.
populasyon.mean()

->popülasyondan 100 tane seçtik. Böylece örneklem alındı.
orneklem = np.random.choice(a=populasyon, size=100)

->Örneklemin ort. Alındı.
orneklem.mean()

############################
# Descriptive Statistics (Betimsel İstatistikler)
############################
->seaborn içerisinden tips veri setini getirdik ve hızlıca bir betimleme işlemi gerçekleştirdik.
df = sns.load_dataset("tips")
->Anlaşılabilecek formatta gözlemledik. Describe metodu veri setindeki sayısal değişkenleri seçerek onları betimler. Bize temel istatistikleri verir.
df.describe().T


############################
# Confidence Intervals (Güven Aralıkları)
############################

# Tips Veri Setindeki Sayısal Değişkenler için Güven Aralığı Hesabı
df = sns.load_dataset("tips")
df.describe().T

df.head()

->sms yani ststmodels içerisinden kullanacak olduğumuz tconfint metodunu kullanarak ilgili güven aralığı hesaplamasını yaptık. 
sms.DescrStatsW(df["total_bill"]).tconfint_mean()
->Bahşişler ile ilgili ortalama aldık.
sms.DescrStatsW(df["tip"]).tconfint_mean()


######################################################
# Correlation (Korelasyon)
######################################################


# Bahşiş veri seti:
# total_bill: yemeğin toplam fiyatı (bahşiş ve vergi dahil)
# tip: bahşiş
# sex: ücreti ödeyen kişinin cinsiyeti (0=male, 1=female)
# smoker: grupta sigara içen var mı? (0=No, 1=Yes)
# day: gün (3=Thur, 4=Fri, 5=Sat, 6=Sun)
# time: ne zaman? (0=Day, 1=Night)
# size: grupta kaç kişi var?

->Ödenen bahşişten toplam hesabı çıkartıyoruz. Böylece iki değişken arasındaki korelasyonu daha sağlıklı değerlendiririz. 
df["total_bill"] = df["total_bill"] - df["tip"]

->Bir saçılım grafiği ile ikisi arasındaki ilişkiyi gözlemleyebiliriz.
df.plot.scatter("tip", "total_bill")
plt.show()

->İkisi arasındaki korelasyona eriştik. Corr metodu iki değişken arasındaki ilişkiyi gözlemlemek istediğimizde kullanılır.
df["tip"].corr(df["total_bill"])


######################################################
# AB Testing (Bağımsız İki Örneklem T Testi)
######################################################
# 1. Hipotezleri Kur
# 2. Varsayım Kontrolü
#   - 1. Normallik Varsayımı ->İlgili iki grubun dağılımlarının normal olması
#   - 2. Varyans Homojenliği-> İki grubun varyanslarının dağılımlarının benzer olması
# 3. Hipotezin Uygulanması
#   - 1. Varsayımlar sağlanıyorsa bağımsız iki örneklem t testi (parametrik test)
#   - 2. Varsayımlar sağlanmıyorsa mannwhitneyu testi (non-parametrik test)
# 4. p-value değerine göre sonuçları yorumla
# Not:
# - Normallik sağlanmıyorsa direk 2 numara. Varyans homojenliği sağlanmıyorsa 1 numaraya arguman girilir.
# - Normallik incelemesi öncesi aykırı değer incelemesi ve düzeltmesi yapmak faydalı olabilir.

############################
# Uygulama 1: Sigara İçenler ile İçmeyenlerin Hesap Ortalamaları Arasında İst Ol An Fark var mı?
############################


df = sns.load_dataset("tips")
df.head()

->sigara içip içmeme durumuna göre groupby alıp tiplerin ort. Aldık. Sonuca göre 1 birimlik fark çıktı ancak istatistiksel olmadığından şu an için kesin gözü ile bakamayız.
df.groupby("smoker").agg({"total_bill": "mean"})

############################
# 1. Hipotezi Kur
############################

# H0: M1 = M2 -> Olası bütün müşterilerimin verisi olsaydı bu müşterilerin ödeyeceği hesap ortalamaları arasında fark yoktur hipotezi kurulmuş oldu.
# H1: M1 != M2 -> Alternatif hipotez.Eşit değildir durumu.

############################
# 2. Varsayım Kontrolü
############################

# Normallik Varsayımı ->Bir değişkenin dağılımının normal dağılıma benzer olup olmadığının hipotez testidir.

# Varyans Homojenliği ->

############################
# Normallik Varsayımı
############################

# H0: Normal dağılım varsayımı sağlanmaktadır.
# H1:..sağlanmamaktadır.

->Test istatistiği ve pvalue değerini shapiro(shapiro testi bir değişkenin dağılımının normal olup olmadığını test eder.) metodu ile normallik sağlanıyor durumunu sağlayıp testi gerçekleştirdik. Sigara içenler yes grubunu ve ilgili değişkeni girdik.
test_stat, pvalue = shapiro(df.loc[df["smoker"] == "Yes", "total_bill"])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

# p-value < ise 0.05'ten HO RED.
# p-value < değilse 0.05 H0 REDDEDILEMEZ.

->Sigara içme durumunu No diyerek testi yaptığımızda 0.05den küçük olduğu için H0 red
test_stat, pvalue = shapiro(df.loc[df["smoker"] == "No", "total_bill"])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))


############################
# Varyans Homojenligi Varsayımı
############################

# H0: Varyanslar Homojendir
# H1: Varyanslar Homojen Değildir

->varsayımımız H0 reddedilmemesi üzerinedir yani varyanslar homojendir durumuna bakıyoruz. Varyans homojenliği varsayımını inclemek için levene testi kullanılır. Levene iki farklı grup ister ve buna göre testi gerçekleştirir.
test_stat, pvalue = levene(df.loc[df["smoker"] == "Yes", "total_bill"],
                           df.loc[df["smoker"] == "No", "total_bill"])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

# p-value < ise 0.05 'ten HO RED.
# p-value < değilse 0.05 H0 REDDEDILEMEZ.

############################
# 3 ve 4. Hipotezin Uygulanması
############################

# 1. Varsayımlar sağlanıyorsa bağımsız iki örneklem t testi (parametrik test) -> yukarıdaki örnekte iki varsayım da sağlanmadı. Örnek olması açısından sağlanmış gibi devam edip diğer aşamaları yapıyoruz.
# 2. Varsayımlar sağlanmıyorsa mannwhitneyu testi (non-parametrik test)

############################
# 1.1 Varsayımlar sağlanıyorsa bağımsız iki örneklem t testi (parametrik test)
############################
->ttesti import ettikten sonra sigara içip içmeme durumlarını girdik. Ttest normallik varsayımı ve varyans homojenliği sağlanıyorsa kullanılır. Bunlardan biri bile sağlanıyorsa kullanılır ancak varyans homojenliği sağlanmıyorsa True yerine False gireriz.
test_stat, pvalue = ttest_ind(df.loc[df["smoker"] == "Yes", "total_bill"],
                              df.loc[df["smoker"] == "No", "total_bill"],
                              equal_var=True)

print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

->Sigara içip içmeme durumuna göre hesapta değişiklik olması durumunda H0 reddedilemedi yani başta 1 fark çıkması şanş eseri
# p-value < ise 0.05 'ten HO RED.
# p-value < değilse 0.05 H0 REDDEDILEMEZ.

############################
# 1.2 Varsayımlar sağlanmıyorsa mannwhitneyu testi (non-parametrik test)
############################
->Az önce sağlanmadığı halde sağlanıyormuş gibi davrandık ancak sağlanmıyorsa non-parametik testi uygulanır. Non-parametik testi ortalama ve medyan kıyaslama testidir. 
->Sonuç olarak H0 reddedilemedi. 
->H1 i kabul etme durumu yoktur sadece H0 ı reddedip reddetmeme durumu vardır. Bu bilgi wikipedia gibi yerlerde bile yanlış verilmektedir. Bunun sebebi de H0 ı reddedeceğimiz durumda yapacak olduğumuz hatayı biliriz ancak bu durum H1 için geçerli değildir.
test_stat, pvalue = mannwhitneyu(df.loc[df["smoker"] == "Yes", "total_bill"],
                                 df.loc[df["smoker"] == "No", "total_bill"])

print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))


############################
# Uygulama 2: Titanic Kadın ve Erkek Yolcuların Yaş Ortalamaları Arasında İstatistiksel Olarak Anl. Fark. var mıdır?
############################

df = sns.load_dataset("titanic")
df.head()
->Öncelikle cinsiyete göre yaş ortalamalarının gropby aldık.
df.groupby("sex").agg({"age": "mean"})

->M ler ana kitle ort. Temsilidir. Örneklemden ana kitleye ilişkin işlem yaptığımızdan dolayı ana kitle yer alır.
# 1. Hipotezleri kur:
# H0: M1  = M2 (Kadın ve Erkek Yolcuların Yaş Ortalamaları Arasında İstatistiksel Olarak Anl. Fark. Yoktur)
# H1: M1! = M2 (... vardır)


# 2. Varsayımları İncele

# Normallik varsayımı
# H0: Normal dağılım varsayımı sağlanmaktadır.
# H1:..sağlanmamaktadır

->shapiro testi ile elimizdeki ile normal dağılım arasında istatistiki bir fark olup olmadığına bakıyoruz. Cinsiyet olarak kadın seçip yaş değişkenini seçtik. Eksik değerler yüzünden arıza olmaması için dropna ile eksiklikleri kaldırdık.
test_stat, pvalue = shapiro(df.loc[df["sex"] == "female", "age"].dropna())
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))
->Aynı işlemi erkekler için gerçekleştirdik.
test_stat, pvalue = shapiro(df.loc[df["sex"] == "male", "age"].dropna())
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

# Varsayımlar sağlanmadığı için nonparametrik
->Bu işlem için mannwhitneyu testi kullanılır. Nonparametrik iki örneklem karşılaştırılır.
->Sonuç olarak frak olduğu ortaya çıktı ve bunu istatistiksel olarak kanıtladık.
test_stat, pvalue = mannwhitneyu(df.loc[df["sex"] == "female", "age"].dropna(),
                                 df.loc[df["sex"] == "male", "age"].dropna())

print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))
# 90 280


############################
# Uygulama 3: Diyabet Hastası Olan ve Olmayanların Yaşları Ort. Arasında İst. Ol. Anl. Fark var mıdır?
############################

df = pd.read_csv("datasets/diabetes.csv")
df.head()

->Yaş ile diyabet arasında bir ilişki olup olmadığını gözlemlemek isteriz. Öncelikle diyabet olup olmama durumuna göre yaşların ort. Aldık.
df.groupby("Outcome").agg({"Age": "mean"})

# 1. Hipotezleri kur
# H0: M1 = M2
# Diyabet Hastası Olan ve Olmayanların Yaşları Ort. Arasında İst. Ol. Anl. Fark Yoktur
# H1: M1 != M2
# .... vardır.

# 2. Varsayımları İncele

# Normallik Varsayımı (H0: Normal dağılım varsayımı sağlanmaktadır.)
test_stat, pvalue = shapiro(df.loc[df["Outcome"] == 1, "Age"].dropna())
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

test_stat, pvalue = shapiro(df.loc[df["Outcome"] == 0, "Age"].dropna())
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))


# Normallik varsayımı sağlanmadığı için nonparametrik.
->nonparametrik karşımıza medyan kıyaslama olarak da çıkabilir.
# Hipotez (H0: M1 = M2)
test_stat, pvalue = mannwhitneyu(df.loc[df["Outcome"] == 1, "Age"].dropna(),
                                 df.loc[df["Outcome"] == 0, "Age"].dropna())
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

###################################################
# İş Problemi: Kursun Büyük Çoğunluğunu İzleyenler ile İzlemeyenlerin Puanları Birbirinden Farklı mı?
###################################################

# H0: M1 = M2 (... iki grup ortalamaları arasında ist ol.anl.fark yoktur.)
# H1: M1 != M2 (...vardır)

df = pd.read_csv("datasets/course_reviews.csv")
df.head()

->Kursun izlenmesi durumuna göre az ve çok izlenmeye göre ortalama aldık.
df[(df["Progress"] > 75)]["Rating"].mean()

df[(df["Progress"] < 25)]["Rating"].mean()


->Hipotezi kurduk ve ilk grup için normallik varsayımını gerçekleştirdik.
test_stat, pvalue = shapiro(df[(df["Progress"] > 75)]["Rating"])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))


test_stat, pvalue = shapiro(df[(df["Progress"] < 25)]["Rating"])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

->Hipotez sağlanmadığından aşağıdaki işlemi yapıyoruz.
test_stat, pvalue = mannwhitneyu(df[(df["Progress"] > 75)]["Rating"],
                                 df[(df["Progress"] < 25)]["Rating"])

print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))


######################################################
# AB Testing (İki Örneklem Oran Testi)
######################################################

# H0: p1 = p2
# Yeni Tasarımın Dönüşüm Oranı ile Eski Tasarımın Dönüşüm Oranı Arasında İst. Ol. Anlamlı Farklılık Yoktur.
# H1: p1 != p2
# ... vardır

->Her iki grubun da başarı sayısını bir arraye gözlem sayılarını da ayrı bir arraye koyduk.
basari_sayisi = np.array([300, 250])
gozlem_sayilari = np.array([1000, 1100])

->Kıyaslama için proportions_ztest metodunu kullanırız. Bu metod 1. Argümanına count yani başarı sayısını 2. Argümana da gözlem sayılarını ister. Pvalue 0.5 den küçük çıktığı için H0 reddedildi. Yani iki oran arasında istatistiki olarak anlamlı bir farklılık vardır.
proportions_ztest(count=basari_sayisi, nobs=gozlem_sayilari)

->1. Grup 0.3 2. Grup 0.22 çıktı yani ilk grup daha başarılı.
basari_sayisi / gozlem_sayilari

############################
# Uygulama: Kadın ve Erkeklerin Hayatta Kalma Oranları Arasında İst. Olarak An. Farklılık var mıdır?
############################

# H0: p1 = p2
# Kadın ve Erkeklerin Hayatta Kalma Oranları Arasında İst. Olarak An. Fark yoktur

# H1: p1 != p2
# .. vardır

df = sns.load_dataset("titanic")
df.head()
->Kadınların hayatta kalma oranı
df.loc[df["sex"] == "female", "survived"].mean()
->Erkeklerin hayatta kalma oranı
df.loc[df["sex"] == "male", "survived"].mean()

->İlk argümana başarı ikinci argümana gözlem sayısını girdik. Sum ederek sayıları elde ettik.
female_succ_count = df.loc[df["sex"] == "female", "survived"].sum()
male_succ_count = df.loc[df["sex"] == "male", "survived"].sum()

test_stat, pvalue = proportions_ztest(count=[female_succ_count, male_succ_count],
                                      nobs=[df.loc[df["sex"] == "female", "survived"].shape[0],
                                            df.loc[df["sex"] == "male", "survived"].shape[0]])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))


######################################################
# ANOVA (Analysis of Variance)
######################################################
->Buradaki problemimiz haftanın günlerine göre ödenen hesaplarda farklılık var mı?
# İkiden fazla grup ortalamasını karşılaştırmak için kullanılır.

df = sns.load_dataset("tips")
df.head()
->Günlerin ortalamaları açısından fark olup olmadığına bakıyoruz.
df.groupby("day")["total_bill"].mean()

# 1. Hipotezleri kur

# HO: m1 = m2 = m3 = m4
# Grup ortalamaları arasında fark yoktur.

# H1: .. fark vardır

# 2. Varsayım kontrolü

# Normallik varsayımı
# Varyans homojenliği varsayımı

# Varsayım sağlanıyorsa one way anova
# Varsayım sağlanmıyorsa kruskal

# H0: Normal dağılım varsayımı sağlanmaktadır.

->Veri setindeki day değişkenini günlere göre filtrelemek istiyoruz.
->for diyerek df in içerisindeki day değişkeninin unique değerlerini getirip bir listeye çevirdik. Böylece üzerinde gezebileceğiz.
->shapiro testi ile günleri seçip totalbill in 1. İndexine bakıyoruz.
->pvalue değerini alıp print ettik.
for group in list(df["day"].unique()):
    pvalue = shapiro(df.loc[df["day"] == group, "total_bill"])[1]
    print(group, 'p-value: %.4f' % pvalue)

# H0: Varyans homojenliği varsayımı sağlanmaktadır.
->levene testini kullanarak 4 grubu girdik.
test_stat, pvalue = levene(df.loc[df["day"] == "Sun", "total_bill"],
                           df.loc[df["day"] == "Sat", "total_bill"],
                           df.loc[df["day"] == "Thur", "total_bill"],
                           df.loc[df["day"] == "Fri", "total_bill"])
print('Test Stat = %.4f, p-value = %.4f' % (test_stat, pvalue))

# 3. Hipotez testi ve p-value yorumu

# Hiç biri sağlamıyor.
df.groupby("day").agg({"total_bill": ["mean", "median"]})


# HO: Grup ortalamaları arasında ist ol anl fark yoktur

# parametrik anova testi:
f_oneway(df.loc[df["day"] == "Thur", "total_bill"],
         df.loc[df["day"] == "Fri", "total_bill"],
         df.loc[df["day"] == "Sat", "total_bill"],
         df.loc[df["day"] == "Sun", "total_bill"])


# Nonparametrik anova testi:
kruskal(df.loc[df["day"] == "Thur", "total_bill"],
        df.loc[df["day"] == "Fri", "total_bill"],
        df.loc[df["day"] == "Sat", "total_bill"],
        df.loc[df["day"] == "Sun", "total_bill"])

->Aralarında fark olduğunu tespit ettikten sonra farklılığın hangisinden kaynaklı olduğunu bulmak için gerekli kütüphane importlarını yaptıktan sonra test istatistiğini elde ediyoruz.
from statsmodels.stats.multicomp import MultiComparison
comparison = MultiComparison(df['total_bill'], df['day'])
tukey = comparison.tukeyhsd(0.05)->Yaygın olan 0.05 dir.
->Sonuçları elde ettik.
print(tukey.summary())

