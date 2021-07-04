# crawlingcovid
Hi, Sintaks ini adalah sintaks untuk crawling data dari beberapa website. Silakan dikembangkan sesuai kebutuhan :)

# Insert Library
    library(httr)
    library(openxlsx)
    library(XML)
    library(tidyverse)

# GLOBAL CASES
    cafile <- system.file("CurlSSL", "cacert.pem", package = "RCurl")
    page <- GET("https://virusncov.com/",
            path="",
            query="",
            config(cainfo = cafile))

    x <- text_content(page)
    tab <- sub('.*(<table class="grid".*?>.*</table>).*', '\\1', x)
    covid_global<-readHTMLTable(tab)

    df<-data.frame(covid_global)
    write.xlsx(df, "path/file name.xlsx")

# INDONESIA CASES
    resp <- GET ("https://data.covid19.go.id/public/api/update.json")
    status_code (resp)
    cov_id_raw <- content(resp, as = "parsed", simplifyVector = TRUE)

    names(cov_id_raw)
    cov_id_update <- cov_id_raw$update
    lapply(cov_id_update,names)

    x<-cov_id_update$harian
    data_update<-data.frame(matrix(unlist(x), ncol=length(x)))

    rename <- rename(data_update, Tanggal=X1, key=X2, doc=X3, Jumlah_meninggal=X4, Jumlah_sembuh=X5, Jumlah_positif=X6, Jumlah_dirawat=X7, Jumlah_positif_kum=X8,
                 Jumlah_sembuh_kum=X9, Jumlah_meninggal_kum=X10, Jumlah_dirawat_kum=X11)
    head(rename)
    
    write.xlsx(rename, "path/file name.xlsx")

# INDONESIA PROVINCE CASES
    resp <- GET ("https://apicovid19indonesia-v2.vercel.app/api/indonesia/provinsi")
    status_code (resp)
    cov_id_raw <- content(resp, as = "parsed", simplifyVector = TRUE)
    head(cov_id_raw)
    
    data_update_prov<-data.frame(matrix(unlist(cov_id_raw), ncol=length(cov_id_raw)))
    head(data_update_prov)
    
    write.xlsx(data_update_prov, "path/file name.xlsx")


