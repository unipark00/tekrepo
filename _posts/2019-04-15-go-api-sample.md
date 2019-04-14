---
title: "sample API server w/ go-server"
search: false
categories:
  - Git
last_modified_at: 2019-04-14T10:00+09:00
---

This post shows how to show git branch on shell prompt.

1) sample api server
```go
// GetPetById - Find pet by ID
func GetPetById(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Content-Type", "application/json; charset=UTF-8")
        w.WriteHeader(http.StatusOK)
        pet := Pet{0, Category{0, "pet1"}, "doggie", []string{"url1", "url2"}, []Tag{{0, "tag1"}}, "available"}
        js, err := json.Marshal(pet)
        if err != nil {
                http.Error(w, err.Error(), http.StatusInternalServerError)
                return
        }
        w.Write(js)
}
```

2) curl
```curl
# url -X GET "http://localhost:8080/v2/pet/findByStatus?status=available" -H "accept: application/xml"
```
