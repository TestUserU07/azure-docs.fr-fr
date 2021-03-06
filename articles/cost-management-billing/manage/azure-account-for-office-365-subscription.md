---
title: S’inscrire à Office 365 avec un compte Azure
description: Apprenez à créer un abonnement à Office 365 avec un compte Azure.
author: JiangChen79
ms.reviewer: adwise
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: banders
ms.openlocfilehash: 736009a88f309e0a38402222668a9609f844552f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79204815"
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Souscrire un abonnement Office 365 avec un compte Azure
Si vous êtes abonné à Azure, vous pouvez utiliser votre compte Azure pour souscrire un abonnement Office 365. Si vous faites partie d’une organisation qui a un abonnement Azure, vous pouvez créer des abonnements Office 365 pour les utilisateurs dans votre instance Azure Active Directory (Azure AD) existante. Inscrivez-vous à Office 365 avec un compte détenant les autorisations Administrateur général ou Administrateur de facturation dans votre locataire Azure Active Directory. Pour plus d’informations, consultez les pages [Vérifier mes autorisations de compte dans Azure AD](#RoleInAzureAD) et [Attribution de rôles d’administrateur dans Azure Active Directory](../../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Si vous avez déjà à la fois un compte Office 365 et un abonnement Azure, vous pouvez [associer un locataire Office 365 à un abonnement Azure](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Obtenir un abonnement Office 365 avec un compte Azure

1. Accédez à la [page de produit Office 365](https://products.office.com/business), puis sélectionnez un plan.
2. Cliquez sur **Se connecter** dans le coin supérieur droit de la page.

    ![Capture d’écran de la page d’essai d’Office 365.](./media/azure-account-for-office-365-subscription/12-office-365-trial-page.png)
3. Connectez-vous avec les informations d’identification de votre compte Azure. Si vous avez créé un abonnement pour votre organisation, utilisez un compte Azure membre du rôle de répertoire Administrateur général ou Administrateur de facturation dans votre client Azure Active Directory.

    ![Capture d’écran de la connexion à Office 365.](./media/azure-account-for-office-365-subscription/13-office-365-sign-in.png)
4. Cliquez sur **Essayez dès maintenant**.

    ![Capture d’écran confirmant votre commande d’Office 365.](./media/azure-account-for-office-365-subscription/14-office-365-confirm-your-order.png)
5. Sur la page du reçu de la commande, cliquez sur **Continuer**.

    ![Capture d’écran de la réception de la commande d’Office 365.](./media/azure-account-for-office-365-subscription/15-office-365-order-receipt.png)

Vous êtes prêt.
Si vous avez créé l’abonnement Office 365 pour votre organisation, suivez les étapes ci-dessous pour vérifier que vos utilisateurs Azure AD sont maintenant dans Office 365.

1. Ouvrez le centre d’administration Microsoft 365.
2. Développez la section **UTILISATEURS**, puis cliquez sur **Utilisateurs actifs**.

    ![Capture d’écran des utilisateurs de centre d’administration Microsoft 365](./media/azure-account-for-office-365-subscription/16-microsoft-365-admin-center-users.png)

Une fois que vous êtes inscrit, l’abonnement à Office 365 est ajouté à l’instance Azure Active Directory à laquelle votre abonnement Azure appartient. Pour plus d’informations, consultez les pages [En savoir plus sur les abonnements Azure et Office 365](office-365-account-for-azure-subscription.md#more-about-subs) et [Association des abonnements Azure avec Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="check-my-account-permissions-in-azure-ad"></a><a id="RoleInAzureAD"></a>Vérifier mes autorisations de compte dans Azure AD
1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Cliquez sur **Tous les services**, puis recherchez **Active Directory**.

    ![Capture d’écran d’Active Directory sur le Portail Azure.](./media/azure-account-for-office-365-subscription/billing-more-services-active-directory.png)
3. Cliquez sur**Utilisateurs et groupes** > **Tous les utilisateurs**.
4. Sélectionnez le nom d’utilisateur.

    ![Capture d’écran montrant les utilisateurs d’Azure Active Directory.](./media/azure-account-for-office-365-subscription/billing-users-groups.png)

5. Cliquez sur **Rôle de répertoire**.

    ![Capture d’écran montrant le rôle de répertoire du Portail Azure.](./media/azure-account-for-office-365-subscription/billing-user-directory-role.png)
6.  Le rôle **Administrateur global** ou **Administrateur limité** > **Administrateur de facturation** est requis pour créer un abonnement Office 365 pour les utilisateurs dans votre instance d’Azure Active Directory existante.

    ![Capture d’écran montrant le rôle de répertoire Administrateur de facturation du Portail Azure.](./media/azure-account-for-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>Vous avez besoin d’aide ? Contactez-nous.

Si vous avez des questions ou besoin d’aide, [créez une demande de support](https://go.microsoft.com/fwlink/?linkid=2083458).
